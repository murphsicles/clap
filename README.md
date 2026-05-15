# @cli/clap — Command Line Argument Parser for Zeta

Faster and more efficient than the Rust original. Single-pass parsing, packed arg storage, no HashMaps in the hot path, zero dynamic dispatch.

## Quick Start

```zeta
use zorb @cli/clap::{Command, Arg, ArgAction};

let matches = Command::new("myapp")
    .version("1.0.0")
    .author("Roy Murphy")
    .about("Does awesome things")
    .add_arg(Arg::new("config")
        .short('c')
        .long("config")
        .help("Sets a config file"))
    .add_arg(Arg::new("verbose")
        .short('v')
        .long("verbose")
        .help("Verbose mode")
        .action(ArgAction::SetTrue))
    .get_matches();

let config: Option<&str> = matches.get_one("config");
let verbose: bool = matches.get_flag("verbose");
```

## API

### Command

| Method | Description |
|--------|-------------|
| `new(name)` | Create a new command |
| `.version(v)`, `.author(a)`, `.about(a)` | Set metadata |
| `.add_arg(arg)` | Add an argument |
| `.subcommand(cmd)` | Add a subcommand |
| `.get_matches()` | Parse `env::args()` (exits on error) |
| `.try_get_matches()` | Parse, return Result |
| `.get_matches_from(iter)` | Parse custom args |

### Arg

| Method | Description |
|--------|-------------|
| `new(id)` | Create with unique ID |
| `.short('v')` | Set short flag |
| `.long("verbose")` | Set long flag |
| `.help("text")` | Help text |
| `.value_name("FILE")` | Placeholder in usage |
| `.action(action)` | What happens on match |
| `.required(true)` | Required flag |
| `.multiple(true)` | Allow multiple values |
| `.index(0)` | Positional at index |
| `.default_value("val")` | Default if not provided |

### ArgAction

| Action | Behavior |
|--------|----------|
| `Set` | Takes a value (default) |
| `Append` | Takes multiple values |
| `SetTrue` | Boolean flag, sets to true |
| `SetFalse` | Boolean flag, sets to false |
| `Count` | Counts occurrences (-vvv) |
| `Help` | Print help and exit |
| `Version` | Print version and exit |

### ArgMatches

| Method | Returns |
|--------|---------|
| `.get_one("id")` | `Option<&str>` |
| `.get_many("id")` | `Vec<&str>` |
| `.get_flag("id")` | `bool` |
| `.get_count("id")` | `u64` |
| `.contains_id("id")` | `bool` |
| `.subcommand()` | `Option<(&str, &ArgMatches)>` |

## Why Faster

- **Single-pass parsing** — no backfilling or second passes
- **Linear scan** over a `Vec<Arg>` — faster than HashMap for < 50 args (no hashing)
- **No macros** — no derive macro compile time cost
- **No dynamic dispatch** — all types are concrete
- **Dense storage** — no separate HashMaps for short/long/positional lookups

## License

MIT
