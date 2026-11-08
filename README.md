# Cinqo tools

This repository holds what you need to run **Cinqo** on your Mac and to add its **tools**:

| In this repository                         | What it is                                                                    |
| ------------------------------------------ | ----------------------------------------------------------------------------- |
| `cinqo-pro`                                | The Cinqo app for a Mac with Apple Silicon on **macOS 26 or newer**.          |
| `cinqo-air`                                | The Cinqo app for a Mac with Apple Silicon on **macOS 12 or newer**.          |
| `repository.json`                          | The list of tools. The app reads it, you never open it yourself.              |
| `browser/`, `career/`, `pdf_tools/`        | The tool packages (`<tool>/versions/tool-<tool>-<system>.zip`).               |

Both apps are the same program, built for different macOS versions. If you are not sure which one to take, take
**`cinqo-air`**: it runs on every Mac that `cinqo-pro` runs on.

## Run the app

You need a Mac with Apple Silicon (M1, M2, M3 …). Google Chrome is recommended: the app opens in it.

1. **Download** `cinqo-air` (or `cinqo-pro`) and put it in a folder of its own, for example `~/Cinqo/`. The app keeps its
   data (database, logs, installed tools) in a `data/` folder **next to the file**, so choose a folder you can write to
   and keep.

2. **Allow it to run.** A file you downloaded is blocked by macOS until you say it is fine. Open Terminal in that folder
   and run, once:

   ```sh
   chmod +x cinqo-air
   xattr -d com.apple.quarantine cinqo-air
   ```

   (A file that was copied to your Mac with `cp`, `scp` or `rsync` has no block, and the second line is not needed.)

3. **Start it:**

   ```sh
   ./cinqo-air
   ```

   The app opens its own Chrome window at <http://localhost:7030>. Sign in with your account on the sign-in service the
   app is set up for (<https://coco-iam.t-digi.com>).

4. **Stop it** by closing that Chrome window, or with `Ctrl+C` in the Terminal. Both shut the app down cleanly.

Good to know:

- To keep the data somewhere else, name the folder before the start: `./cinqo-air --data ~/cinqo-data start`.
- The app uses the ports **7030** (the page you open) and **7026** (its backend). If a Cinqo is already running from the
  same folder, starting it again stops the old one first.
- No Chrome installed? The app opens your normal browser instead. The Browser tool still needs Chrome (see below).
- "Built for a newer version of macOS": you started `cinqo-pro` on an older Mac. Use `cinqo-air`.

## Add the tools

The app already knows this repository: it is its **built-in tool repository**, so the tools in it are offered without
you adding anything.

1. In the app, open the tool repositories page: <http://localhost:7030/admin/tool-repositories>. The list should show
   this repository, loaded from
   `https://raw.githubusercontent.com/a-digi/cinqo-tools/main/repository.json`.
2. Choose a tool from it and install it. Install **Browser** and **PDF Tools** first, or at the same time: **Career
   needs both**.
3. When a new version of a tool is published here, the same page offers the update.

### The tools

| Tool          | What it does                                                                                                                                                                                              | Needs                  |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| **Career**    | Your career workspace: profiles, personas, skills, experience and education; job platforms that are crawled for jobs; matching a job with your profile; importing a CV into your profile; generating a CV as PDF. | Browser and PDF Tools  |
| **Browser**   | Opens and reads web pages for the other tools, with Chrome. When a site asks you to prove you are human, it shows a window so that you can.                                                                | Google Chrome          |
| **PDF Tools** | Reads a PDF as text and generates PDFs.                                                                                                                                                                   | nothing                |

The packages in this repository are for macOS (Apple Silicon), Linux (amd64 and arm64) and Windows (amd64). The apps in
this repository are for macOS only.

### AI in the tools

Career uses an AI model for the job match, the crawl instructions, the CV generation and the CV import. In the app:

- **the AI menu of the app**: add the key of a provider (OpenAI, Anthropic or OpenRouter), or
- **Local AI** (<http://localhost:7030/admin/local-ai>): let Cinqo run a language model on your own computer. The first
  time you switch it on, the app downloads and installs the program that runs the model, and a model of your choice, in
  the background.

Then choose the platform and the model once in **Career → Settings**. It is used everywhere in Career.

A small local model cannot do everything a large one can. For the CV import, **Phi-4-mini** works (the app does the
steps itself and asks it only small questions), **Qwen2.5-1.5B** is too small and the import warns you before it starts.

## Problems

| What you see                                              | What to do                                                                                          |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| macOS says the file is damaged or from an unknown source  | Run `xattr -d com.apple.quarantine cinqo-air` once (step 2 above).                                  |
| The page does not open, or the port is busy               | Something else uses 7030 or 7026. Close it and start again.                                         |
| The Tool repositories page shows an error for this list   | The list could not be loaded: check your internet connection. Tools you installed keep working.     |
| Career does not offer itself to install                   | Install **Browser** and **PDF Tools** first.                                                        |

## For whoever publishes here

Everything in this repository is made in the Cinqo source repository and copied here:

1. `make build-app-all` builds `cinqo-pro` and `cinqo-air` (in `app/`).
2. `make zip` in a tool folder builds that tool's packages.
3. `make repository-in-place` writes `tools/repository.json` with the links to this repository.
4. `make repository-copy` copies the packages and `repository.json` here. The two apps are copied by hand.

Then commit and push. The links in `repository.json` only work for files that are pushed to the `main` branch.
