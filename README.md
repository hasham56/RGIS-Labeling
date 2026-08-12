# Tesco price-ticket labeling — read this first

We are teaching a computer to find **Tesco shelf-edge price tickets** in shelf
photos, and to tell how big each ticket physically is. Your job is to outline every
price ticket in your photos and mark it **small**, **medium** or **large**.

Each of you has a folder of photos:

| Folder | Photos |
|---|---|
| `abdul_dataset/` | 30 |
| `jara_dataset/` | 38 |
| `hasham_dataset/` | 21 |

`original_dataset/` holds 43 photos that are **already labeled to standard** — import
that first and look at it before you start.

---

## 0. Get the files

The photos are too big for GitHub, so they arrive separately. You need both halves in
the **same folder**.

1. **Get this guide and the config files.** On the repo page click
   **Code → Download ZIP**, and unzip it somewhere sensible — that folder is your
   working folder for everything below. (`git clone` works too if you use git.)
   It contains `README.md`, `label_config.xml` and `original_dataset_tasks.json`.
2. **Get your photos.** Hasham will send you two Google Drive links — download both:
   - `original_dataset.zip` — the finished reference photos (everyone gets this)
   - `<your name>_dataset.zip` — your own share
3. **Unzip both inside the folder from step 1**, so it ends up looking like this:

```
RGIS-Labeling/
├── README.md
├── label_config.xml
├── original_dataset_tasks.json
├── original_dataset/        <- from original_dataset.zip
└── abdul_dataset/           <- from your own zip (jara_dataset / hasham_dataset)
```

The folder names matter — Label Studio is pointed at them by name later, so unzip
in place and do not rename anything.

## 1. Setup (once)

You need **Python 3.9, 3.10 or 3.11**. Label Studio does not install cleanly on 3.12
or newer — if you already have a newer Python, install 3.11 alongside it rather than
fighting it.

### Windows

1. Install Python 3.11 from <https://www.python.org/downloads/release/python-3119/>
   (the "Windows installer (64-bit)"). On the first screen **tick "Add python.exe to
   PATH"** before clicking Install.
2. Open **PowerShell**, go to this folder, and create the environment:

```powershell
cd C:\path\to\RGIS_Labeling
py -3.11 -m venv ls-venv
.\ls-venv\Scripts\python -m pip install --upgrade pip
.\ls-venv\Scripts\pip install label-studio
```

That last step takes a few minutes. If `py -3.11` is not recognised, Python was
installed without the PATH tickbox — re-run the installer and choose Modify.

### macOS / Linux

```bash
cd /full/path/to/RGIS_Labeling
python3 -m venv ls-venv
./ls-venv/bin/pip install --upgrade pip
./ls-venv/bin/pip install label-studio
```

On Ubuntu, if `venv` complains, run `sudo apt install python3-venv` first.

### Start Label Studio — every time, like this

Both variables are mandatory. Without them every photo shows as a broken image.

**Windows (PowerShell):**

```powershell
cd C:\path\to\RGIS_Labeling
$env:LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED="true"
$env:LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT="C:\path\to\RGIS_Labeling"
.\ls-venv\Scripts\label-studio
```

**macOS / Linux:**

```bash
cd /full/path/to/RGIS_Labeling
export LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true
export LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/full/path/to/RGIS_Labeling
./ls-venv/bin/label-studio
```

Replace the path with wherever this folder actually sits on your machine — on Windows
copy it from the File Explorer address bar, on macOS/Linux run `pwd` inside the folder.
The rest of this guide writes paths in the `/full/path/to/RGIS_Labeling/...` style; on
Windows use your `C:\...\RGIS_Labeling\...` form instead, with backslashes.

Windows may pop up a firewall prompt the first time — click **Allow access**; this is
only Label Studio serving to your own browser.

Then open <http://localhost:8080> and create an account. It is local to your machine,
so any email and password work — nothing is sent anywhere. Leave that window running
while you label; closing it shuts Label Studio down (your work is saved).

---

## 2. Import the original dataset (the reference)

This is the finished example: 43 photos, 1,610 tickets already outlined and sized.
You are not editing it — you are looking at it to see what "done properly" means.

1. **Create Project** → call it `Reference`.
2. **Settings → Labeling Interface → Code** → delete what is in the box, paste the
   entire contents of `label_config.xml` → **Save**.
3. **Settings → Cloud Storage → Add Source Storage → Local files**
   - **Absolute local path**: `/full/path/to/RGIS_Labeling/original_dataset`
   - **Test Connection** → **Add Storage**
   - **Do not click Sync.**
4. Go to the task list → **Import** → select `original_dataset_tasks.json` → 43 tasks
   appear, each showing its tickets already outlined and colour-coded by size.

**Do step 2 before step 4.** If you import first, every region shows as "unknown
label" and the only fix is deleting the project and starting again.

Open a few tasks. Note how tightly the outlines hug the printed card, and which
things are deliberately *not* outlined.

---

## 3. Label your own share

### Set the project up

1. **Create Project** → call it your name.
2. **Settings → Labeling Interface → Code** → paste `label_config.xml` → **Save**.
   (Same file as before. Hotkeys: `1` small, `2` medium, `3` large, `4` price_label.)
3. **Settings → Cloud Storage → Add Source Storage → Local files**
   - **Absolute local path**: `/full/path/to/RGIS_Labeling/<your name>_dataset`
   - **Test Connection** → **Add Storage**
4. This time **click Sync**. Your photos become tasks automatically — there is no
   JSON to import, because your photos have no labels on them yet.

> Never use the **Upload Files** button in the Import dialog. It renames every photo
> with a random prefix, and filenames are how all our work gets merged back together.

### What counts as a price ticket

**Outline these** — the printed card in the rail along the front edge of a shelf that
shows a price:

- Any size, from a big `£2.50` ticket to a tiny one far back on a high shelf
- Any price format — `£1.40`, `90p`, per-kg, was/now
- Plain white tickets **and** yellow *Clubcard Price* tickets
- Red *Everyday Low Price* tags, as long as they show a price
- Tickets on the next-door bay, seen at an angle
- Partly hidden tickets — if you can tell it is a price ticket at all, outline the
  visible part

**Do not outline these:**

| Not a price ticket | Why |
|---|---|
| Blue *"This shelf is not for customer use"* strips and their `1 case high` boxes | stocking notices, no price |
| Location codes like `19L 01G` with a barcode | warehouse position codes |
| Decorative bands — *"5-A-DAY"*, *"100 Calories"*, brand slogans | no price on them |
| Clip strips of hanging products | merchandise, not tickets |
| Prices printed on packaging, `08 JUL` date stickers | not shelf-edge tickets |

The single test: **is there a price on it, and is it sitting in the shelf rail?**

### How to draw

Outline the **printed paper ticket only** — not the clear plastic holder, and never
the long metal rail that runs the length of the shelf. Hug the edge of the card: no
big margin of shelf around it, and do not cut off the price.

Photos are taken from the side, so tickets usually look like **slanted** rectangles.
Follow the four real corners of the card. A leaning four-cornered shape is correct.

### How to pick the size

Judge the ticket's **real physical size** — use the shelf rail and the tickets next to
it as your ruler. Do **not** judge by how big it looks in the photo: a large ticket at
the far end of the aisle still looks tiny.

**If you are not sure, press `4` (price_label) and move on.** Do not guess. "Unsure"
is a real answer that gets sorted out later against Tesco's official ticket
dimensions; a wrong guess quietly damages the training data. About 1 in 12 of the
labels in `original_dataset/` are marked unsure — that is normal, not a failure.

Small tickets are genuinely rare and mostly turn up in wide shots of a whole aisle.
`IMG_4168.jpg` in the reference project is the best example of what they look like.

### Working through it

Use **Label All Tasks** to go photo by photo. Draw the outline, then press `1`, `2`,
`3` or `4` for its size. **Submit every task with `Ctrl+Enter`** — anything not
submitted does not exist as far as the export is concerned.

---

## 4. Export when you are done

1. Check the task list shows every one of your photos as **completed**. Unsubmitted
   tasks are silently dropped from the export.
2. **Export** (top right) → choose **JSON** — the full JSON, *not* JSON-MIN.
3. Save it as `<your name>.json` and send it to Hasham.

JSON is the format that keeps the polygon points, the size labels and the original
filenames together. JSON-MIN drops detail we need, and the other formats lose the
link back to the photo filenames.

---

## If something looks wrong

| Symptom | Cause |
|---|---|
| Photos show as broken images | Label Studio was not started with the two environment variables, or the storage path in step 3 is wrong |
| Regions say "unknown label" | the labeling config was pasted *after* importing — delete the project and redo it in order |
| Duplicate empty tasks appeared | Sync was clicked on the Reference project — only your own project gets synced |
| Photos have long random names | the Upload button was used instead of storage sync |

Anything else, or any photo you genuinely cannot judge — ask Hasham rather than
guessing.
