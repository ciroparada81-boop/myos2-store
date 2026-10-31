# MyOS Store repository

This is a public HTTPS catalog for MyOS 2's Store. Add the catalog URL below in
**Store → Repositories**:

```text
https://raw.githubusercontent.com/ciroparada81-boop/myos2-store/main/index.txt
```

The Store currently supports MyScript source files (`.my`), not native ELF
programs or archives. It downloads each source over HTTPS, checks the SHA-256
listed in `index.txt`, and saves it under `/home/store`.

## Catalog format

Each non-comment line has exactly six pipe-separated fields:

```text
id|name|version|description|https-download-url|sha256
```

Package IDs contain lowercase letters, digits, hyphens, or underscores.
Descriptions and other fields must not contain `|`. Package URLs must be
direct HTTPS links ending in `.my`; redirects are not supported. Source files
must be at most 128 KiB.

SHA-256 detects a mismatch between the catalog and the downloaded file; it is
not a digital signature. Review scripts before running them: MyScript programs
can access files in your `/home`.

To publish a package, add its `.my` source under `apps/`, calculate its digest:

```sh
sha256sum apps/example.my
```

Then add or update the corresponding line in `index.txt` and push the changes
to the `main` branch.

## Included programs

- **Hola MyOS** — a simple welcome window.
- **Paleta** — choose colors with the mouse.
- **Pong** — a small two-player game controlled with W/S and the arrow keys.
