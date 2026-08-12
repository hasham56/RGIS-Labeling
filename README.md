# Tesco price-ticket labeling

Outline every shelf-edge price ticket in your photos and mark it **small**, **medium**
or **large**.

| Folder | Photos |
|---|---|
| `abdul_dataset/` | 30 |
| `jara_dataset/` | 38 |
| `hasham_dataset/` | 75 |

`original_dataset/` is 43 photos already labeled — your reference.

---

## 0. Get the files

1. On the repo page: **Code → Download ZIP**, unzip it. That folder is your working
   folder. It has `README.md`, `label_config.xml`, `original_dataset_tasks.json`.
2. From the [Drive folder](https://drive.google.com/drive/folders/10PVs_fygZLiWEdH9MrJ9a3bQn6qbvGIe?usp=sharing)
   download two files: `original_dataset.zip` and your own `<name>_dataset.zip`.
3. Unzip both **inside** the folder from step 1:

```
RGIS-Labeling/
├── README.md
├── label_config.xml
├── original_dataset_tasks.json
├── original_dataset/
└── abdul_dataset/
```

Do not rename anything.

## 1. Setup

Python **3.9, 3.10 or 3.11** (3.12+ will not install Label Studio).

**Windows** — install Python 3.11 from
[python.org](https://www.python.org/downloads/release/python-3119/), ticking
**"Add python.exe to PATH"**. Then in PowerShell:

```powershell
cd C:\path\to\RGIS-Labeling
py -3.11 -m venv ls-venv
.\ls-venv\Scripts\pip install label-studio
```

**macOS / Linux:**

```bash
cd /path/to/RGIS-Labeling
python3 -m venv ls-venv
./ls-venv/bin/pip install label-studio
```

### Start it — every time, like this

Both variables are required, or photos show as broken images.

```powershell
cd C:\path\to\RGIS-Labeling
$env:LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED="true"
$env:LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT="C:\path\to\RGIS-Labeling"
.\ls-venv\Scripts\label-studio
```

```bash
cd /path/to/RGIS-Labeling
export LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true
export LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/path/to/RGIS-Labeling
./ls-venv/bin/label-studio
```

Open <http://localhost:8080> and create an account — it is local, any email works.
Leave the terminal running while you label.

---

## 2. Import the reference (43 labeled photos)

1. **Create Project** → `Reference`.
2. **Settings → Labeling Interface → Code** → paste `label_config.xml` → **Save**.
3. **Settings → Cloud Storage → Add Source Storage → Local files** →
   path `<folder>/original_dataset` → **Add Storage**. Do not Sync.
4. **Import** → `original_dataset_tasks.json` → 43 labeled tasks.

Paste the config before importing, or every region shows as "unknown label".

---

## 3. Label your own share

1. **Create Project** → your name.
2. **Settings → Labeling Interface → Code** → paste `label_config.xml` → **Save**.
   Hotkeys: `1` small, `2` medium, `3` large, `4` price_label.
3. **Settings → Cloud Storage → Add Source Storage → Local files** →
   path `<folder>/<name>_dataset` → **Add Storage**.
4. Click **Sync**. Your photos become tasks — no JSON to import.

Never use **Upload Files**; it renames every photo and breaks the merge.

---

## 4. Export

1. Confirm every task shows as **completed** — unsubmitted tasks are dropped.
2. **Export → JSON** (not JSON-MIN).
3. Save as `<name>.json` and send it to Hasham.
