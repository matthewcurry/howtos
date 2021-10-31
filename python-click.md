# Click

The [Click library](https://click.palletsprojects.com) helps you write
command-based CLIs (like the git cli) by adding decorators to simple Python
functions.  Here we'll set up a simple example which is a command-line wrapper
around some functions in the `arrow` library.

## Prerequisites

First, let's set up an executable script.  There are a number of ways to do
this at varying levels of abstraction, but it is helpful to use a tool to
manage your dependencies and assist with packaging.  We will use
[poetry](https://python-poetry.org/).

```
# Create a project
poetry new click_example && cd click_exmaple
```

Inside the source subfolder (somewhat confusingly named `click_example` as
well), create `cli.py` with the following content.

```python
def cli():
  print('Hello, world!')
```

Add the following to `pyproject.toml` to define an entry point:

```toml
[tool.poetry.scripts]
click = "click_example.cli:cli"
```

After running `poetry install`, you can now run `poetry run pytest` to run the
(vacuous) included unit test, and `poetry run click` to run your sample script.  Installing Click is then as safe and easy as running

```
poetry add click
poetry add arrow
```

## Commands

Here is a complete python source for the command-line utility, in `cli.py`.

```python
import arrow
import click


@click.group()
@click.option('--utc / --local', default=False, help = "TZ for output")
@click.pass_context
def cli(ctx, utc):
    ctx.ensure_object(dict)
    ctx.obj['tz'] = 'utc' if utc else 'local'


@cli.command(help="Print information about the timestamp.")
@click.argument("timestamp", type=click.INT)
@click.pass_context
def timestamp(ctx, timestamp):
    orig_timestamp = timestamp
    for prefix in ["seconds", "millis", "micros", "nanos"]:
        if 1e9 < timestamp < 1e10:
            click.echo("Looks like %s." % (prefix,))
            dt = arrow.get(timestamp).to(ctx.obj['tz'])
            click.echo("%s (%s)" % (dt, dt.humanize()))
            break
        timestamp /= 1000
    else:
        click.echo("Could not parse %d as a timestamp." % (orig_timestamp,))


@cli.command(help="Print the current datetime, and the timestamp.")
@click.pass_context
def now(ctx):
    dt = arrow.now()
    click.echo(dt.to(ctx.obj['tz']))
    click.echo("%d seconds" % (dt.timestamp(),))
```

* The command group `cli` is the entry point for the application.  It can
  receive options for all other commands nested under it; here I declared that
  the user can supply a `--utc` or `--local` option.
* `ctx.ensure_object(dict)` makes sure that `ctx.obj` exists; if not, it is
  created as a dictionary.  I can use `ctx.obj` to pass any information I need
  to subcommands, so I insert the expected timezone of the output.  The
  `@click.pass_context` decorator tells click to pass in the context as the
  first parameter.
* Two commands are defined in the group: `timestamp` which takes an integer
  argument, and `now` which takes no argument. In both methods, `click.echo` is
  used instead of `print` because it handles some edge cases well, but it also
  has a number of features one may wish to use eventually.

Anyone who has installed the utility can now use it this way.

```
% click
Usage: click [OPTIONS] COMMAND [ARGS]...

Options:
  --utc / --local  TZ for output
  --help           Show this message and exit.

Commands:
  now        Print the current datetime, and the timestamp.
  timestamp  Print information about the timestamp.
% click now
2021-10-31T15:07:29.753647-07:00
1635718049 seconds
% click --utc now
2021-10-31T22:07:31.763093+00:00
1635718051 seconds
% click timestamp 1635718051
Looks like seconds.
2021-10-31T15:07:31-07:00 (16 seconds ago)
% click timestamp 1635718051000
Looks like millis.
2021-10-31T15:07:31-07:00 (20 seconds ago)
% click --utc timestamp 1635718051000
Looks like millis.
2021-10-31T22:07:31+00:00 (28 seconds ago)
```
