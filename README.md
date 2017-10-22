# Eventbrite clone automated tests

This is the repo for the testing program of the MGT656/MGT660 class project.
The class project is a horrible clone of [Eventbrite](https://www.eventbrite.com).

## Important first caveats

Please note, it is possible (indeed, likely) that we will discover bugs in the
testing code, or make changes to the project requirements mid-project. If that
happens, we'll release different versions of the testing code and we'll use
only the latest version when grading your project at the end of the semester.

## Running the tests

You can download binaries/executables for the testing program here:

* Binaries coming soon...

After you download the binary that corresponds to your operating system, you'll
need to make the code executable (likely `chmod` or the Windows equivalent). The
code runs the tests by automating a browser via the
[WebDriver](https://www.w3.org/TR/webdriver/) protocol. When I completed the
assignment, I used either [Selenium stand-alone
server](http://www.seleniumhq.org/download/) or
[ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/) as my
interface to Chrome. You'll need to download those programs and run them too.
What's going to happen is as follows

1. you'll run your team's app *somewhere* (like Cloud9 or Heroku);
2. you'll run selenium or chromedriver on your own computer;
3. you'll run this testing program on your own computer, telling it where your
   project is running (ie. its URL) and where selenium/chromedriver is running
   (its URL).

Then, this testing program will interact with your website, verifying that
it performs the functions we wish it to---for example, it allows us to RSVP
for events. It will do this by opening up a browser window and acting
like a user would, click on certain parts of the page it expects to find,
submitting forms, etc.

### Detailed steps

To start Selenium, run a command like

```
selenium-server -port 4444
```

or to start ChromeDriver, a command like

```
chromedriver --port 4444
```

Clearly, you can use whatever port you want. Then, you run the test code like this

```
eventbrite-clone-selenium-tests "http://localhost:4444/wd/hub" "http://localhost:8000"
```

if you are using selenium or

```
eventbrite-clone-selenium-tests "http://localhost:4444" "http://localhost:8000"
```

if you are using ChromeDriver (with which you don't need `/wd/hub`). That will run
the tests against your app running on port `8000` on `localhost`. You can, of
course, point it *anywhere*, including Cloud9 or Heroku. In my experience,
ChromeDriver is a little faster than Selenium.

## Expected output

When all the tests pass, you should see this.

```
Home page:
😎  PASS - looks 💯  with Bootstrap CSS
😎  PASS - has a header
😎  PASS - has a footer
😎  PASS - footer links to home page
😎  PASS - footer links to about page
😎  PASS - has your team logo
😎  PASS - shows a list of events
😎  PASS - individual events link to details and show time
😎  PASS - has a link to the new event page

About page:
😎  PASS - layout is correct
😎  PASS - has your names
😎  PASS - shows your headshots

Event 0:
😎  PASS - uses bootstrap
😎  PASS - has a header
😎  PASS - has a footer
😎  PASS - has a link to the about page in footer
😎  PASS - has a link to the home page in footer
😎  PASS - has a title
😎  PASS - has a date
😎  PASS - has a location
😎  PASS - has an image
😎  PASS - has a list of attendees
😎  PASS - has a form to RSVP
😎  PASS - should not allow RSVP with invalid email
😎  PASS - should not allow RSVP with non-yale email
😎  PASS - should allow RSVP with normal yale email
😎  PASS - should allow RSVP with scrambled yale email
😎  PASS - should allow RSVP with another scrambled yale email

Event 1:
😎  PASS - uses bootstrap
😎  PASS - has a header
😎  PASS - has a footer
😎  PASS - has a link to the about page in footer
😎  PASS - has a link to the home page in footer
😎  PASS - has a title
😎  PASS - has a date
😎  PASS - has a location
😎  PASS - has an image
😎  PASS - has a list of attendees
😎  PASS - has a form to RSVP
😎  PASS - should not allow RSVP with invalid email
😎  PASS - should not allow RSVP with non-yale email
😎  PASS - should allow RSVP with normal yale email
😎  PASS - should allow RSVP with scrambled yale email
😎  PASS - should allow RSVP with another scrambled yale email

Event 2:
😎  PASS - uses bootstrap
😎  PASS - has a header
😎  PASS - has a footer
😎  PASS - has a link to the about page in footer
😎  PASS - has a link to the home page in footer
😎  PASS - has a title
😎  PASS - has a date
😎  PASS - has a location
😎  PASS - has an image
😎  PASS - has a list of attendees
😎  PASS - has a form to RSVP
😎  PASS - should not allow RSVP with invalid email
😎  PASS - should not allow RSVP with non-yale email
😎  PASS - should allow RSVP with normal yale email
😎  PASS - should allow RSVP with scrambled yale email
😎  PASS - should allow RSVP with another scrambled yale email

New event page:
😎  PASS - layout is correct
😎  PASS - has a form for event submission
😎  PASS - has a correctly labeled title field
😎  PASS - has a correctly labeled image field
😎  PASS - has a correctly labeled location field
😎  PASS - has a labeled year field with correct options
😎  PASS - has a labeled month field with correct options
😎  PASS - has a labeled day field with correct options
😎  PASS - has a labeled hour field with correct options
😎  PASS - has a labeled minute field with correct options
😎  PASS - should not allow event with no title
😎  PASS - should not allow event with too-long title
😎  PASS - should not allow event with no image
😎  PASS - should not allow event with bad image
😎  PASS - should not allow event with no location
😎  PASS - should not allow event with too-long location
😎  PASS - should allow event with legal parameters

API:
😎  PASS - should return valid JSON
😎  PASS - should be searchable

✅  Passed: 79
❌  Failed: 0
```

## Prose explanation of the test requirements

Here's what we're testing your app for:

All pages:
* Every page should look good - *really good*. We can't check for tastefulness
  in an automated fashion, so instead we'll test for That means including the
  Bootstrap CSS stylesheet in the `head` of the page -- we'll be looking for a
  `link` with an `href` that contains `bootstrap`.
* Every page should have a `header`.
* Every page should have a `footer` with links to the home page and about page.
* Grading note: we're defining 'every page' as `/`, `/about`, `/events/new`,
  `/events/0`, `/events/1`, and `/events/2`.

Home page:
* The home page should have your logo -- we'll look for an `img` tag with the
  `id` 'logo'.
* The home page should also have a link to the new event page with the `id` 'new'.
* Finally, the home page should have a list of events (a `ul` list, to be specific).
  Each event's `li` should have:
    * The `class` 'event' and the `id` 'event-x', where x is the event's id
      number.
    * A link (an `a` element) with the `id` 'title' that links to the event's
      detail page
    * A `time` tag with the event's date and time

About page:
* The about page should have all team members' names in a `span` with the `id`
  'class-nickname-name', where class-nickname is, well, your class nickname
  (check your profile on the course website if you're confused). Don't forget
  the dashes.
* Additionally, we'll look for a picture of each team member with the `id`
  'class-nickname-headshot'. (Note that this picture doesn't need to actually
  be of the member -- we're not using computer vision to make sure you have
  decent headshots or anything -- but you do need an image for each member.)

New event page:
* The new event page should have a form that `POST`s its data. It needs to have
  fields for `title`, `image`, `location`, `year`, `month`, `day`, `hour`, and
  `minute` as follows (make sure that you match these names exactly, otherwise
  the tests will fail):
    * Each form element should have a `name` attribute set to its name
      (`title`, etc.).
    * Each form element should be labeled with a `label` element with a `for`
      attribute set to the form element's name.
    * `year`, `month`, `day`, `hour`, and `minute` should all be dropdown menus.
      `year` should only have 2017 and 2018 as options, `month` should have
      all of the months' names (not numbers), `day` should have 1-31 (we're
      not checking for valid month/day combinations, though that would be a
      great extension), `hour` should be 0-23, and `minute` should only have
      00 and 30 as options.
    * The submit button for the form should have its `name` attribute set to
      'submit'.
* Errors for new event creation are:
    * No title, image, or location
    * Title or location longer than 50 characters
    * Image with a file extension other than `.png`, `jpg`/`jpeg`, or `.gif`
    * Image that doesn't exist online
* You'll want to show an error message if any of the conditions above are met.
  We'll be looking for them in a `ul` with the `class` 'form-errors.

Event detail pages:
* Event detail pages should have the event's title in a `h1` element with the
  `id` 'title', as well as the date and location in `span` elements with the
  `id`s 'date' and 'location', respectively. The event's image should have the
  `id` 'image'.
* Event detail pages should also have a `ul` list of the attendees with the
  `id` attendees. Each attendee (represented by their email) should be in a
  list element with the class `list-group-item`. (Note that this list might
  not show up if you don't have any attendees yet, so for grading purposes
  it's probably a good idea to RSVP to your own events. Isn't that good
  form, anyway?)
* Finally, event detail pages should have a `POST` form to RSVP. The input in
  this form should have `name`, `id`, and `type` all set to 'email', and the
  submit button should have `name` set to 'submit'.
    * This form should validate the email that you give it to make sure that
      it's a valid yale.edu email. However, it should accept any weird
      capitalizations as long as they're Yale emails, so something like
      `kYle.JeNseN@yAlE.eDu` is acceptable (yes, this is just like that
      SpongeBob meme).

API:
* You should have an API running at `/api/events` that returns a valid JSON of
  every event that your app knows about. It should look like this:
```
    {events:
        [
            {
                id: 0,
                title: 'Hello world',
                location: 'New Haven',
                attendees: [
                    'jacob.bendicksen@yale.edu',
                ],
                image: 'http://yaleherald.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-27-at-11.51.10-AM.png',
                time: '9/19/17 23:30'
            },
            <more events here, maybe>...
        ]
    }
```
* Grading note: the JSON reader that we're using for the tests is pretty
  strict, so make sure that your API responses are structured exactly like this.
* If a `search` parameter is attached to the API request (something like
  `/api/events?search=hello`), you should only return events with the search
  term in the title. If there aren't any, return a JSON that looks like:
    * `{events: []}`

Note that while this document is our best effort at translating the tests into
plain English, the grading code (most in `tests.go` in this repo) is the final
source of truth. It's available to you to look through if you'd like (it's
written in Go, which shouldn't be too hard to understand if you're feeling okay
about JavaScript), and at the end of the day, your grade will be determined by
how many of the tests you're passing in the grading code, rather than their
descriptions here. We're happy to walk you through the grading code in office
hours if you're confused!

## Additional MGT660 requirements

If you are in MGT660, in addition to the automated tests, your code will be
manually graded for the degree to which it conforms to the following
requirement:

* Your code uses a [PostgreSQL](https://www.postgresql.org) database for
  storage of your models.
* Your code structure conforms as much as possible to the
  [12factor app](https://12factor.net) principles (the starter code already)
  does.
* Your code is not vulnerable to SQL injections, CSRF, or XSS.
* Your git history demonstrates effective use of git, particularly informative
  commits, a branching strategy, tagged releases (for example, at the end of
  sprints), and effective documentation.

## Tips

* All of the tests are in the `tests.go` file in this repository.
* The CSS selectors we are using to interact with your app are in the
  `selectors.go` file. You'll need to structure your HTML and CSS such that
  the code passes.
* Use the `-fast` flag if you want to save some time.
* If you want to alter this code, you may! In particular, if you find a bug or
  have an enhancement, I hope you'll send me a pull request. Simply clone with
  repo into your `$GOPATH`. On my system, this code lives at `/Users/kljensen/go/src/github.com/yale-mgt-656/eventbrite-clone-selenium-tests`.
  Then run `go get` and `go build`. This was built with go1.8.3.
