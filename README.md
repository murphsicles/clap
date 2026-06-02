# @cli/clap

Full command-line argument parser for Zeta, ported from Rust's `clap` v4.6.1.

## Usage

```zeta
use clap::Command;

let matches = Command::new("myapp")
    .about("Does awesome things")
    .arg(clap::arg!(--output <FILE>).help("Output file"))
    .get_matches();
```

## License

MIT © Zeta Foundation
