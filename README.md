## Github actions for use in packages I maintain

These actions are tailored to how things are done in these packages.  Using them
unmodified for your own packages likely won't work.  I am aware that this could
be done using less boilerplate but given certain contrains beyond my control,
this is how I prefer to do it for now.

## Used third-party actions

- [actions/checkout@v3](https://github.com/actions/checkout)
- [aws-actions/configure-aws-credentials@v1](https://github.com/aws-actions/configure-aws-credentials)
- [purcell/setup-emacs@master](https://github.com/purcell/setup-emacs)

## Generate and distribute manuals

Used by [`borg`], [`epkg`], [`forge`], [`ghub`], [`magit`], [`magit-section`],
[`transient`] and [`with-editor`].  Results can be found
[here](https://magit.vc/manual/) and [here](https://emacsmirror.net/manual/).

Usage:

```yaml
name: manual
on:
  push:
    branches: master
jobs:
  manual:
    name: "Generate and distribute manual"
    runs-on: ubuntu-latest
    steps:
      - name: Configure
        uses: magit/actions/config@main
      - name: Install Emacs
        uses: magit/actions/install-emacs@main
      - name: Install Texlive
        uses: magit/actions/install-texlive@main
      - name: Install Org
        uses: magit/actions/install-org@main
      - name: Generate manual
        uses: magit/actions/manual-generate@main
      - name: Commit manual
        uses: magit/actions/manual-commit@main
      - name: Publish manual
        uses: magit/actions/manual-publish@main
        with:
          key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          secret: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

## Generate and distribute statistics

Generate repository statistics using the aging
[gitstat](https://github.com/hoxu/gitstats) tool.

Used by [`borg`], [`epkg`], [`forge`], [`ghub`], [`magit`],
[`transient`] and [`with-editor`].  Results can be found
[here](https://magit.vc/stats/) and [here](https://emacsmirror.net/stats/).

Usage:

```yaml
name: stats
on:
  push:
    branches: master
jobs:
  manual:
    name: "Generate and distribute statistics"
    runs-on: ubuntu-latest
    steps:
      - name: Install gitstats
        uses: magit/actions/install-gitstats@main
      - name: Generate statistics
        uses: magit/actions/stats-generate@main
      - name: Publish statistics
        uses: magit/actions/stats-publish@main
        with:
          key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          secret: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

## Generate and distribute page

Used by [emacsair.me], [emacsmirror.net] and [magit.vc].

Usage:

```yaml
name: publish
on:
  push:
    branches: master
jobs:
  manual:
    name: "Generate and distribute page"
    runs-on: ubuntu-latest
    steps:
      - name: Generate page
        uses: magit/actions/page-generate@main
      - name: Publish page
        uses: magit/actions/page-publish@main
        with:
          key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          secret: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

[`borg`]:          https://github.com/emacscollective/borg
[`epkg`]:          https://github.com/emacscollective/epkg
[`forge`]:         https://github.com/magit/forge
[`ghub`]:          https://github.com/magit/ghub
[`magit`]:         https://github.com/magit/magit
[`magit-section`]: https://github.com/magit/magit
[`transient`]:     https://github.com/magit/transient
[`with-editor`]:   https://github.com/magit/with-editor

[emacsair.me]:     https://emacsair.me
[emacsmirror.net]: https://emacsmirror.net
[magit.vc]:        https://magit.vc
