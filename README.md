## (Opinionated) Github actions to generate and distribute manuals

These actions are used to generate and publish the manuals for the
[`borg`], [`epkg`], [`forge`], [`ghub`], [`magit`], [`magit-section`],
[`transient`] and [`with-editor`] packages.  Maybe this can serve as
inspiration but because it is tailored to how my manuals are authored
and distributed, using this unmodified for your own manuals likely
won't work.  I am aware that this could be done using less boilerplate
but given certain contrains beyond my control, this is how I prefer to
do it for now.

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

[`borg`]:          https://github.com/emacscollective/borg
[`epkg`]:          https://github.com/emacscollective/epkg
[`forge`]:         https://github.com/magit/forge
[`ghub`]:          https://github.com/magit/ghub
[`magit`]:         https://github.com/magit/magit
[`magit-section`]: https://github.com/magit/magit
[`transient`]:     https://github.com/magit/transient
[`with-editor`]:   https://github.com/magit/with-editor
