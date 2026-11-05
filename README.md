# memogarden-mbox-importer

Import emails from mbox files (e.g. Google Takeout exports) into the
MemoGarden Soil database as immutable Email facts.

Part of [MemoGarden](https://github.com/memogarden) — a personal memory
system for financial transactions.

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
