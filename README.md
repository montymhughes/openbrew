# openbrew

A plain-text coffee journal and stats tracker for the terminal.
Inspired by [hledger](https://hledger.org/) and the plain-text accounting movement.

```
$ openbrew stats
openbrew stats
────────────────────────────────────────
  Beans tracked:    4
  Total brews:      12
  Favorite method:  v60 (5 brews)
  Avg rating:       4.0/5 (12 rated)
  Brews per week:   3.2

  Top beans:
    Ethiopia Yirgacheffe        3  ███
    Colombia Huila              3  ███
    Espresso Blend No. 3        3  ███
    Kenya Nyeri AA              3  ███

  Methods:
    v60                    5 (42%)  ████████
    espresso               3 (25%)  █████
    aeropress              2 (17%)  ███
    french-press           1 (8%)  █
    chemex                 1 (8%)  █
```

## Why?

- **Plain text is forever.** Your data is a simple, human-readable text file. No databases, no lock-in, no proprietary formats.
- **Hackable.** It's a single Python file. Read it, modify it, extend it. Adding a new field is a few lines of code.
- **Unix-friendly.** Pipe it, grep it, version-control it.
- **No dependencies.** Just Python 3.6+ standard library.

## Install

```bash
git clone https://github.com/montymhughes/openbrew.git
cd openbrew
chmod +x openbrew
```

Then either add it to your PATH:

```bash
mkdir -p ~/.local/bin
ln -s $(pwd)/openbrew ~/.local/bin/openbrew
```

Or just run it directly:

```bash
./openbrew --help
```

## Quick start

```bash
# Create your journal
openbrew init

# Add your first bag of beans
openbrew add bean

# Log a brew
openbrew add brew

# See your stats
openbrew stats

# Try it with the included example data
openbrew -f example.journal stats
openbrew -f example.journal timeline
openbrew -f example.journal top
```

## Commands

```
add bean          Interactively add a new bean
add brew          Interactively add a new brew
beans             List all beans with brew counts and avg ratings
brews             List all brews in a table
stats             Overall statistics — top beans, methods, averages
timeline          Visual timeline of recent brews with ratings
top               Rank beans by average rating
search <query>    Full-text search across all entries
tags              List all tags and how often they're used
methods           Brew method breakdown
finish            Mark a bean as finished (hides from brew picker)
sync              Sync active beans list (for iOS Shortcuts integration)
import <file>     Import entries from another journal file
init              Create a new empty journal
edit              Open your journal in $EDITOR
help format       Show the journal file format reference
```

### Filtering brews

```bash
openbrew brews --bean "Ethiopia"
openbrew brews --method v60
openbrew brews --since 2025-01-15 --until 2025-02-01
openbrew brews -n 5
```

## Journal format

Your journal is a plain UTF-8 text file. Entries are separated by blank lines. Each entry starts with a date and a type (`bean` or `brew`), followed by indented fields.

```
2025-01-15 bean "Ethiopia Yirgacheffe"
  roaster: Sweet Bloom
  origin: Ethiopia
  process: washed
  roast: light
  roast_date: 2025-01-10
  price: $18.50
  weight: 340g
  varietals: Heirloom
  +single-origin, +fruity
  ; Incredible blueberry and jasmine aroma

2025-01-16 brew "Ethiopia Yirgacheffe"
  method: v60
  dose_in: 18g
  dose_out: 290ml
  grind: 22
  water_temp: 96°C
  brew_time: 3:15
  rating: 4/5
  +morning, +pourover
  ; Bright and fruity, maybe grind finer next time
```

### Format rules

- Dates are `YYYY-MM-DD`
- Entry names are in double quotes
- Fields are indented with 2+ spaces or a tab
- Tags start with `+` and are comma-separated
- Notes start with `;`
- Rating is 1–5 (write `4` or `4/5`)
- All fields except date and name are optional
- Lines starting with `#` or `;` outside entries are comments
- Field names are flexible — `temp` works the same as `water_temp`, `in` works the same as `dose_in`

### Bean fields

| Field | Aliases | Example |
|-------|---------|---------|
| roaster | | `Sweet Bloom` |
| origin | | `Ethiopia` |
| process | | `washed`, `natural`, `honey` |
| roast | roast_level | `light`, `medium`, `dark` |
| roast_date | | `2025-01-10` |
| price | | `$18.50` |
| weight | | `340g`, `12oz` |
| varietals | | `Heirloom`, `SL28` |
| finished | archived, done | `yes` |

### Brew fields

| Field | Aliases | Example |
|-------|---------|---------|
| method | | `v60`, `aeropress`, `espresso` |
| dose_in | in, dose | `18g` |
| dose_out | out, yield | `290ml`, `36g` |
| grind | | `22` |
| water_temp | temp, water | `96°C` |
| brew_time | time | `3:15` |
| rating | | `4/5` |

## Configuration

Set your journal location with an environment variable:

```bash
export OPENBREW="$HOME/path/to/your/journal.journal"
```

Or pass it per command:

```bash
openbrew -f /path/to/journal stats
```

### Environment variables

| Variable | Purpose |
|----------|---------|
| `OPENBREW` | Path to your journal file (default: `~/.openbrew.journal`) |
| `EDITOR` / `VISUAL` | Used by `openbrew edit` |
| `NO_COLOR` | Disables coloured output when set |

## iOS Shortcuts integration

You can log brews from your phone using Apple Shortcuts. openbrew maintains an `active-beans.txt` file alongside your journal — a plain list of bean names, one per line — which Shortcuts can read to build a dropdown picker.

1. Store your journal in iCloud Drive
2. Point openbrew at it: `export OPENBREW="$HOME/Library/Mobile Documents/com~apple~CloudDocs/openbrew.journal"`
3. Run `openbrew sync` to generate the initial `active-beans.txt`
4. In Shortcuts, create a shortcut that:
   - Uses **Get File** to read `active-beans.txt` from iCloud Drive
   - Uses **Split Text** by new lines, then **Choose from List** for the bean picker
   - Prompts for method, dose, grind, rating, etc.
   - Formats it as a journal entry and **Appends to File** (`openbrew.journal`)

The active beans list updates automatically when you add or finish beans from the terminal.

## Tips

**Version control your journal:**

```bash
cd ~/openbrew && git add openbrew.journal && git commit -m "Brews this week"
```

**Use grep for quick lookups:**

```bash
grep "rating: 5" ~/.openbrew.journal
grep "method: espresso" ~/.openbrew.journal
```

**Sync across devices** with iCloud Drive, Syncthing, Dropbox, or git.

**Adding new fields** is easy — the pattern is always: add a property to the class, a line in the parser, a line in the serializer, and optionally an interactive prompt. The codebase is designed to make this straightforward.

## Contributing

Contributions are welcome. openbrew is intentionally simple — a single Python file with no dependencies. If you'd like to add a feature, open an issue first to discuss it.

## License

MIT — do whatever you want with it.
