# memogarden-mbox-importer

Import emails from mbox files (e.g. Google Takeout exports) into the
MemoGarden Soil database as immutable Email facts.

Part of [MemoGarden](https://github.com/memogarden) — a personal memory
system for financial transactions.

## What's new in 0.1.1

- The dedup index is now built from a **projected SQL scan** (`json_extract`
  of just the message id) instead of materializing every Email fact as a
  parsed Python object — index construction memory is bounded (~tens of MB)
  instead of scaling with the corpus (~2 GB transient at 65k emails).
- Deduplication is **account-qualified** (RFC-012 D1): the same message
  delivered to two mailboxes lands as two facts; the dedup key is
  `(account, Message-ID)`. Threading stays global by Message-ID.

## Install

Expects the MemoGarden workspace layout (sibling checkouts):

```bash
git clone https://github.com/memogarden/memogarden-system.git
git clone https://github.com/memogarden/memogarden-mbox-importer.git
cd memogarden-mbox-importer
poetry install
```

## Usage

```python
from system.soil import get_soil
from mbox_importer.import_mbox import MboxImporter

soil = get_soil()
with soil:
    soil.init_schema()

importer = MboxImporter(soil=soil, mbox_path="takeout.mbox")
stats = importer.import_mbox(limit=10)
print(stats)
```

## License

MIT
