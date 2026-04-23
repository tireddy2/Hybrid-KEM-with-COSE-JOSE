<!-- regenerate: off (set to off if you edit this file) -->

# COSE HPKE PQ &amp; PQ/T Algorithm Registrations

This is the working area for the individual Internet-Draft, "COSE HPKE PQ &amp; PQ/T Algorithm Registrations".

* [Editor's Copy](https://tireddy2.github.io/cose-hpke-pqt-pqc/#go.draft-reddy-cose-hpke-pq-pqt.html)
* [Datatracker Page](https://datatracker.ietf.org/doc/draft-reddy-cose-hpke-pq-pqt)
* [Individual Draft](https://datatracker.ietf.org/doc/html/draft-reddy-cose-hpke-pq-pqt)
* [Compare Editor's Copy to Individual Draft](https://tireddy2.github.io/cose-hpke-pqt-pqc/#go.draft-reddy-cose-hpke-pq-pqt.diff)


## Contributing

See the
[guidelines for contributions](https://github.com/tireddy2/cose-hpke-pqt-pqc/blob/main/CONTRIBUTING.md).

The contributing file also has tips on how to make contributions, if you
don't already know how to do that.

## Command Line Usage

Formatted text and HTML versions of the draft can be built using `make`.

```sh
$ make
```

Command line usage requires that you have the necessary software installed.  See
[the instructions](https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md).

## Regenerating Examples and Draft Sections

The draft markdown uses HTML comment markers (e.g.,
`<!-- begin:... ; see README for regeneration instructions, do not edit -->` /
`<!-- end:... -->`) to delimit auto-generated sections. Content between
matching begin/end pairs is overwritten by the script and should not be
edited directly in the markdown.

The algorithm definitions, COSE test vectors, and several draft sections
(algorithm tables, IANA registrations, test vectors appendix) are generated
by automation that is maintained in the companion JOSE repository and invoked
through this repository's `package.json` scripts.

The generated outputs must be committed to the repository. After modifying the
algorithm set or other generated content, run the following locally and commit
the results:

```sh
# requires Node.js >=24.7.0 and network access for `npx`
node --run update
```

This will:

1. Regenerate `examples/cose-keys/` and `examples/cose/` directories
2. Regenerate `examples/cose-vectors.json`
3. Update the draft markdown with regenerated algorithm identifier tables,
   IANA registration entries, and test vector sections

The current `update` script delegates to automation hosted in the companion
JOSE repository, which contains the shared algorithm definitions and
generation logic.

To force regeneration of examples regardless:

```sh
node --run force-update
```
