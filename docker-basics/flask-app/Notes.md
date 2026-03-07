# Dockerfile: COPY vs ADD

Both commands are used to copy files from the host machine into the Docker image, but they have distinct differences.

## COPY

The `COPY` instruction is straightforward and explicit. It copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.

**Features:**

- Copies local files/directories.
- Does **not** support remote URLs.
- Does **not** auto-extract archives.

**Best Practice:**

- Use `COPY` for the majority of your needs (e.g., copying source code, config files). It is more transparent about what is happening.

## ADD

The `ADD` instruction is more feature-rich but can be unpredictable if you aren't careful.

**Features:**

- Copies local files/directories.
- **Remote URLs:** Can download a file from a URL and copy it to the destination.
- **Auto-Extraction:** If the source is a local tar archive (identity, gzip, bzip2 or xz), it automatically unpacks it into the destination directory.

**When to use:**

- Only use `ADD` if you specifically need to download a file or auto-extract a tarball.
