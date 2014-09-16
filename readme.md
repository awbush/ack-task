# Ack Task

## Overview

Ack task converts grep and ack output (file:line:matching text) into HTML links
so you can click them to open the file. It:

- keeps track of clicked links
- provides a second set of checkboxes for custom tracking
- has a Notes dropdown for those occasions where tracking some notes with the
  search results helps.
- works with ack, grep, Sublime Text, TextMate, and more.

## Installation

Put `ackt` somewhere in your PATH, e.g. `/usr/local/bin/` or `~/bin/`.

For example:

	curl https://raw.githubusercontent.com/awbush/ack-task/master/ackt > ackt
	chmod a+x ackt
	test -f /usr/local/bin/ackt || sudo mv ackt /usr/local/bin/

## Configuration

Links to external editors are supported in the form:

	$HANDLER://open/?url=$FILE&line=$LINE

The default is to open the file using `file://` protocol.  Set the
`ACKT_HANDLER` environment variable or use `--link=HANDLER` on the command line
to customize it to something else (e.g. [txmt][mate] for TextMate, [subl][subl]
for Sublime Text).

For example, to use Sublime Text by default:

	echo "export ACKT_HANDLER=subl" >> ~/.bash_profile
	. ~/.bash_profile

### Advanced config

If you want be reminded of the search command used, specify the `--cmd="..."`
arg.  If you don't need to perform piped searches (as done with the longer
examples above), you can make a shortcut that does this for you.  Consider
this in your `~/.bash_profile` file:

    function acktc() { tmpAck="$@"; ack "$@" | ackt --cmd="ack $tmpAck"; }

Then you could use this all-in-one for searching and displaying HTML results:

    acktc -i something

The only difference is that the without `--cmd` you'll see "unknown command" in
the HTML output. Not a big deal if you remember what you ran. TODO: I'd love
for this to be auto-detected; if anyone knows how to parse the usage examples
below without needing this option, submit a pull request!

## Usage Examples

Did you grep?

    grep -rin something | ackt
    grep -rin something | grep -v not_this | ackt

Did you ack?

    ack -i something | ackt
    ack -i something | ack -v not_this | ackt

Works with most `file:line:text` output:

    pcregrep -rinM "\..*\(\s*something" | ackt

## TODO

- Support more than OS X?
- FreeBSD / MIT license?
- PHP? Really? It's already written... so... yes, for now.

[mate]: http://blog.macromates.com/2007/the-textmate-url-scheme/
[subl]: https://github.com/asuth/subl-handler
