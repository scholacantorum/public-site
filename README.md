---
noindex: true
---

# Schola Cantorum Public Web Site

This repository contains the content of the
[scholacantorum.org](https://scholacantorum.org) web site.  Changes to the web
site content are made through this repository.

## Access and Permissions

Look at the top right corner of this page.  If it says “Sign in or Sign up”,
then you need to do that first.  If you already have a github.com account,
sign into it.  If you don’t, sign up for one (it’s free).  If you don’t see
“Sign in or Sign up”, then you’re already signed in.

Next, you need permissions to be able to edit the site.  If you don’t know
whether you have permissions:

1. Click on the right-most menu in the black bar at the top of the page.  In
   that menu, click on “Your profile”.
2. Look on the left-hand side of the page, under your name.  If you see an
   “Organization” heading, and underneath it the Schola Cantorum logo, then
   you have permissions.
3. If you don't have permissions, open the right-most menu again.  The first
   thing in the menu should be “Signed in as «yourname»”.  Make a note of
   _exactly_ how «yourname» is spelled, and send it in an email to Steve Roth,
   requesting permission.

## How to Make Changes

The directory tree in this repository matches the menu structure of the site.
(It’s described in more detail below.)

If you want to change a page on the site, navigate through the directory tree
to the file containing the content for that page.  Just above the page content,
you’ll see a bar with a light gray background.  On the right side of that bar
are “Raw”, “Blame”, and “History” buttons.  To the right of those buttons is a
pencil icon.  Click on the pencil icon to edit the page.

While editing the page, you can click on the “Preview changes” tab to see what
your changes look like after formatting.  Click on the “Edit file” tab to
continue making changes.  Switch back and forth as often as you want.

When you are finished editing the page, find the “Commit changes” heading under
the editor.  Immediately underneath it is a pair of entry fields where you
should describe what change you made.  This is not required, but it can be very
helpful later when looking at the history of the page.  After that, click on the
green “Commit changes” button.

If you want to create a new page, navigate to the directory that it belongs in,
and click the “Create new file” button near the top right.  Give it a name, and
then edit and save the page the same way as above.

If you want to delete a page, navigate to the file containing the content for
that page, and click the trash can icon (right next to the pencil).

If you want to add a file that isn't a page — like an image that will be shown
on a page, or a PDF file that vill be referenced by a page — navigate to the
directory that it belongs in, and click on the “Upload files” button near the
top right.

**In all of these cases,** your changes will be applied immediately to the
“sandbox” site at [new.scholacantorum.org](https://new.scholacantorum.org).
You should visit that site to be sure your changes work as you intend.  The
sandbox site looks exactly like the production site except for the pink banner
at the top.  Note that ticket purchases, donations, etc. on the sandbox site are
fake; they don't actually charge any card.

After testing your changes on the sandbox site, you can publish them to the
production site ([scholacantorum.org](https://scholacantorum.org)) by entering
the publish password and clicking the “Publish” button in the pink bar.  (If
you don’t know a publish password, have someone who does know it review your
changes and publish them for you.)

## Page Formatting

Most pages of the site are written in Markdown format, which is plain English
with some simple formatting rules:

* `*word*` gives an italicized *word*
* `**word**` gives a bold-face **word**
* `# Heading` gives a page heading
* `## Heading` gives a second-level heading (etc.)
* `[Hyperlink text](url)` gives a hyperlink to the URL
* `![image name](image-filename)` embeds an image
* `---` (three or more hyphens) gives a horizontal line
* Paragraphs are separated by blank lines

Here's a [cheat sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) with lots of other things you can do in Markdown.  And, if you’re familiar with HTML, you can also put raw HTML into the file.  Keep
that to a minimum, though; it will will be hard for others to maintain.

## Directory Layout

The `concerts`, `events`, `donations`, and `membership` directories correspond
to the “Concerts”, “Events”, “Donate”, and “Sing” pages in the main menu.  The
`about`, `leadership`, `mailing-list`, `outreach`, `policies`, `press`, and
`volunteering` directories correspond to the various pages in the drop-down
menu.  The `_index.md` file is the home page.

In the `concerts` and `events` directories, the `_index.md` file is the body
text of the page that lists the concerts and events, respeoctively.  Then there
is a subdirectory for each individual concert or event.  In those
subdirectories, you will find these files:

* `index.md` is the main body of the page for the concert or event.  Note that
  it has settings at the top for the `title` of the concert or event, and the
  `weight`, which says what order the concerts or events come in.

* `summary.md` is the short descriptive text for the concert or event, that
  appears on the page tha lists all of the concerts or events.

* `image.png` or `image.jpg` is the image for the event.  It should be square
  (more or less), at least 300px on a side.  Don’t worry about resizing it
  exactly; the site builder will do that.

* `seating.md` (or `seating1.md` and `seating2.md`) describe when and where the
  concert or event occurs, and how to get tickets for it.  These have settings
  at the top for `datetime` and `venue`.  `datetime` has to have the format
  `YYYY-MM-DD HH:MM:SS`, in 24-hour time.  (You can leave out the time entirely
  if it’s not known.)  `venue` has to be one of the known venue codes, currently
  `fccpa`, `laumc`, `memchu`, and `mvcpa`.  Contact Steve Roth if you need a new
  one added.  You can leave out the `venue` entirely if it’s not known.

  The text of `seating.md` is the ticket ordering for the seating.  If tickets
  aren’t available, you can put a message here saying so.  If they’re available
  at some other site (e.g. MVCPA box office), you can put a link to that site
  here.  If we’re selling tickets on this site, you can put the ticket order
  form here.

## Special Codes

There are some special codes that are used on some pages.

`{{% sponsor %}} Fred Flintstone {{% /sponsor %}}` renders a “Concert sponsor:”
(or “Event sponsor:”) line with a larger font and special coloring.  You can
use “sponsors” if there's more than one.

`{{% youthnote %}}` adds the standard note about youth under 25 getting in free
with student ID.  It has special styling.

## Questions?

If you have questions, contact Steve Roth.
