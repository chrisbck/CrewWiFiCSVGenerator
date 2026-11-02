# Crew Wi-Fi CSV Generator

A single static web page that turns a pasted crew list into a CSV ready to bulk-import user accounts into an AntLabs IG4 gateway, and prints credential slips to hand out.

No build step, no dependencies, no server — open `index.html` in a browser. Nothing leaves the page; the CSV is generated locally.

## Usage

1. Paste the crew list into the text box, one person per line: **name**, then **position**.
   Separate the two with a **tab** (copying two columns from Excel does this) or **at least two spaces**.
   ```
   Bob Dylan	Welder
   Kylie Ann Minogue	Roustabout
   ```
2. Choose the **Plan** (defaults to `Regular Crew 28 Days`).
3. Click **Generate Table** to preview names, positions and usernames.
4. Click **Download CSV** and import the file into AntLabs.
5. Click **Print Credentials** to print cut-out slips (username, password, position).
6. **IG4 Login** opens the AntLabs admin page (`https://10.105.2.93/admin/index-login.ant`).

Download and Print are disabled until the table is generated, and again whenever the input is edited, so the CSV and printed slips always match.

Lines with no position (e.g. name and position separated by a single space) are **skipped** and listed in a yellow warning above the table — fix them and regenerate.

## Generated values

| Field | Value |
|---|---|
| User ID | Initials + surname joined with underscores: `Kylie Ann Minogue` → `K_A_Minogue` |
| Password | 7 random characters from a set that excludes look-alikes (`l I 1 O o 0`) |
| Plan | Chosen from the dropdown: `Regular Crew 28 Days` (default) or `VTG starlink time based 30 GB plan` |
| Zones | `2` |
| Description | The crew member's position |
| Valid From / Date | Time of download, `DD/MM/YYYY hh:mmAM` |
| Creator | `admin` |

The plan options, zone and the IG4 address are hard-coded in `index.html`; edit them there if they change.

## Duplicate usernames — intended behaviour

- **Within one batch:** if two people produce the same username, the second gets a number appended (`B_Dylan1`) and its row is highlighted pink.
- **Against existing AntLabs accounts:** the tool deliberately does **not** check or avoid these. When a returning crew member's username already exists, the AntLabs import throws an error; that error is the cue to delete the old account and re-import. This is by design — the available AntLabs permissions don't allow a better workflow — so don't add suffixes or other uniqueness tricks that would suppress the error.

## Known limitations

- Usernames keep only ASCII letters, digits and dots: accents, apostrophes and hyphens are stripped (`O'Brien` → `Obrien`, `José` → `Jos`).
- The CSV filename date uses UTC, so very early local-time downloads may be named with the previous day's date.
