
## Run

    brew install hugo
    hugo server --buildDrafts

## Build

    # produces ./public/
    hugo --gc --minify

## Write

Embed raw html:

    {{< rawhtml >}}
    <lol>
    {{< /rawhtml >}}

## Admin

List drafts:

    hugo list drafts | grep -o '^[^,]\+'

## Update deps

    brew install hugo

# Theme

https://github.com/adityatelange/hugo-PaperMod/wiki/Installation
