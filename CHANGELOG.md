# Version 12.1.1

#### FIXED:

* Fixed an issue with unintentional "broadcasting" of `numpy` arrays in certain
  `'expression'` questions.

* Fixed an issue that prevented `datetime` and `timedelta` objects from being
  logged.


# Version 12.1.0

#### ADDED:

* Added questions' types and grading modes to the `'question_info'` cache.

* `datetime` and `timedelta` objects (from the `datetime` module) are now
  allowed in log entries.

#### CHANGED:

* The automatic source code download no longer saves cached copies, to avoid
  accidentally filling small disks.

* Upgraded KaTeX to version 0.9.0.

* Upgraded SweetAlert2 to version 7.15.1.

* Upgraded MathJax to version 2.7.3.

* Upgraded PLY to version 3.11.

#### REMOVED:

* `'questions'` is no longer a valid key for `tutor.compute_page_stats`, as it is
  redundant with `'question_info'`.

#### FIXED:

* Fixed an issue where invalid question names were failing silently.

* Prevented scroll bars from showing up in the CAT-SOOP logo at the bottom of
  the page.

* The `'question_info'` cache is now checked/updated after the pre_handle plugin
  is executed, rather than after.

* `'expression'` questions now support implicit multiplication by `j` (the
  imaginary unit).

* Fixed an issue that caused scores in the `'problemstate'` log to be overwritten
  with `None` when the "check" button was used.

* Entering an empty expression and entering an invalid expression to a
  `'pythonic'` question now produce the same error message.

* Fixed a bug where API token was not being properly set when impersonating a
  user.

* In the default handler, ensure `handle_check` creates all problemstate fields
  if they don't already exist.

* The checker script is now aware of the `cs_now` variable.

* Fixed an issue that caused a misleading error message when the remote Python
  sandbox couldn't be reached.

* Fixed a misleading error message about how to install `jose` when using the
  `'openid_connect'` authentication type.

* Fixed a broken link after an error message when using the `'openid_connect'`
  authentication type.

#### SECURITY:

* Prevented `as_role`  from affecting the `'role'` field in `cs_user_info` if the
  user does not have the `'impersonate'` permission.


# Version 12.0.0

#### ADDED:

* Added a cache of question information, to avoid having to fake a page load
  to get question information in `tutor.compute_page_stats`.

#### CHANGED:

* CAT-SOOP can optionally be configured to use uWSGI, and the options for
  configuring the WSGI server have changed.

* Logs are now stored in plain-text files, as opposed to binary log file.

* Page views are no longer logged in the `'problemactions'` log.

* Updated KaTeX to 0.9.0-beta1.

* `'question_points'` is no longer a valid key in `tutor.compute_page_stats`.
  `'question_info'` should be used instead.

#### FIXED:

* Fixed a bug with rendering of scores when manual grades are submitted.


# Version 11.1.2

#### ADDED:

* Added "Log In" / "Log Out" to top menu by default, and gave authentication
  types the ability to populate that menu.

#### CHANGED:

* Separated WSGI server into separate process, and allow `cs_wsgi_server_port` to
  be a list to start multiple servers (for load balancing).

* Updated `wsgi.py` so that it can be used by an external WSGI server (such as
  uWSGI).

* `csq_nosubmit_message` is now sent only on an actual submission (not when
  viewing the page), so that the submit button still appears when loading pages
  with `cs_nosubmit_message` set.

* Buttons are now always created under `'view'` mode in the default handler.

* `cs_nosubmit_message` was replaced with `csq_nosubmit_message` (specific to
  each question).

#### FIXED:

* Fixed an issue where checker results were stored in an invalid location in
  the case of some errors.

* Fixed an issue with the checker updating scores for problems that don't yet
  have a `'problemstate'` log.

* Added CSS to prevent scrollbars from appearing on the CAT-SOOP logo in page
  footers.

* Fixed key generation for AES encryption in `'login'` authentication mode.

* Fixed a bug with changing password in `'login'` authentication mode.

* Fixed a bug with client-side password hashing so that the hash is now a
  proper PBKDF2 hash of the given password.

* The `'last_submit_time'` and `'last_submit_times'` entries in the problemstate log
  are now only updated if the submission was successful.

* Fixed a bug whereby some compound expressions were treated as literals in the
  `'pythonliteral'` question type.

* Fixed a bug with rendering of `<` and `>` in the `'richtext'` question type.

* Fixed an issue with empty `<python>` tags.

* Fixed a broken link in the OpenID Connect login page.


# Version 11.1.1

#### ADDED:

* Added the `'legacy'` grading mode, which does not use the asynchronous checker.

#### CHANGED:

* Separated checker results into subdirectories to prevent a single directory
  growing too large.

* Helper scripts are now launched using sys.executable, rather than a
  hard-coded `"python3"`.

* Checks run through the asynchronous checker now cache their results in the
  problemstate directory, to avoid having to do a double-lookup.

* Attempts at improved error reporting throughout CAT-SOOP.

#### REMOVED:

* Removed the `PKiller` class, which is no longer used anywhere.

#### FIXED:

* Temporary workaround for automatic scrolling to location of anchor.

* Fixed a bug with username/password-based access to the `get_user_information`
  API endpoint.

* Fixed a bug with the name of uploaded files.

#### SECURITY:

* The current session ID and API token are now filtered from error messages.

#### DOCUMENTATION:

* Added (hopefully) meaningful docstrings to almost everything in the codebase.


# Version 11.1.0

#### ADDED:

* Added cheroot (<https://github.com/cherrypy/cheroot>) as a `catsoop.tools`
  package.

* Added websockets (<https://github.com/aaugustin/websockets>) as a `catsoop.tools`
  package.

#### CHANGED:

* cheroot is now used instead of uWSGI.

* File names for uploaded files now include the `csq_name` of the question they
  were uploaded for.

#### FIXED:

* Fixed an issue whereby the Python sandbox could leave handles to deleted
  files open (eventually leading to an error from having too many files open).

* Changed the Python sandbox to use `sys.executable` (instead of hard-coded path
  to an assumed location of a Python interpreter) when `csq_python_interpreter` is
  not set.

#### SECURITY:

* File names for uploaded files now include a hash of their contents to limit
  the feasibility of a brute-force attack to grab a different user's files.


# Version 11.0.5

#### ADDED:

* Added the `get_upload` utility for downloading uploaded files.

* Added `'extra_data'` field to checker results.

* Added `tutor.read_checker_result` for quickly reading checker results.

* Added `csq_npoints` as an option to the `'pythoncode'` question type.  If
  specified, it overrides any point values assigned to the individual test
  cases.

#### CHANGED:

* Changed the default checker used for multiple-choice-multiple-answer problems.

* The `'fileupload'` question type now uses `get_upload` to download user files,
  rather than a data URI.

* Made changes to make sure that files are always closed after being opened.

#### FIXED:

* Fixed a potential race condition in the `'pythoncode`' question type.

* Fixed an issue with the `'expression'` question type, where overzealous type
  checking led to some correct answers being marked as incorrect.

* Fixed issues with the circuit simulator question type that prevented an AC
  analysis from being run interactively, and that caused incorrect AC results
  to be sent to the checker.

* Changes to make sure that reasonable fonts are used (particularly in `<pre>`,
  `<code>`, and `<tt>`) even if our fonts are not loaded.

* Some small temporary fixes to AC analysis in the circuit simulator for the
  `'circuit'` question type.

#### SECURITY:

* Fixed an issue whereby `get_upload` could be used to read arbitrary files from
  disk via a carefully constructed request.

# Version 11.0.4

#### FIXED:

* Fixed issue with matrix comparison in expression question type.

* Fixed typo (`SyntaxError`) in handout handler.

# Version 11.0.3

#### FIXED:

* Use <https://github.com/aaugustin/websockets> for websocket connections.

#### REMOVED:

* Removed the included simple websockets implementation.

* Remove more traces of the abandoned queue.


# Version 11.0.2

#### FIXED:

* Moving a file from `'running'` to `'results'` in the checker is now implemented
  as an atomic operation, to avoid the potential for corrupted results files.


# Version 11.0.1

#### FIXED:

* Changes to the way the `'reporter'` process handles websocket connections.


# Version 11.0.0

#### ADDED:

* Added script to migrate SQLite logs (and checker) to new format.

#### CHANGED:

* Logs are now, once again, stored in `catsoopdb` format.  No other options.

* The checker's state is now stored via the filesystem, rather than through
  SQLite.

* File upload paths are now stored relative to `cs_data_root` to make moving
  between systems easier.  Old (absolute) paths still work, but the system will
  now store relative paths.

* Results from completed checks are now loaded in a way that avoids requiring a
  websocket connection.

* The checker and reporter are now two separate processes.

#### FIXED:

* Fixed serious bug (deadlock) with checker script / logging.

* Fixed HTML rendering of pythoncode question type.

# Version 10.4.0

#### ADDED:

* Added a means of viewing a page with different (lower) permissions by
  spoofing your role (the `as_role` query string argument).

* Added a `'dummy'` auth type to make authentication on local testing setups
  easier.

* Added preliminary support for running on MacOS and Windows (Cygwin) hosts by
  fixing a number of Mac-specific issues.

* The `'score'` display can now be changed on a per-problem basis via
  `csq_score_message`.


#### CHANGED:

* pycs compiled files are now stored in `<cs_data_root>/_cached`, to avoid
  polluting the course itself.

* The body of the CAT-SOOP default template is now slightly wider.

* Local Python sandboxes now write their `stdout` and `stderr` to files instead of
  relying on low-level hacks involving pipes.

* Updated KaTeX to version 0.8.2.

* Updated python-markdown to version 2.6.9.

* The magic variable controlling the checker's location is now
  `cs_checker_websocket` instead of `cs_websocket_location` (which is now
  ambiguous).

* Added more options to modify the styling of the "login required" page.

* Cross/check images are now preceded by a newline.

#### DEPRECATED:

* `cs_score_message` has been deprecated, in favor of `csq_score_message`.
  `cs_score_message` will be removed in a future version.

#### REMOVED:

* Removed lots of unused imports, unused local variables, etc, courtesy of
  pyflakes.

#### FIXED:

* Fixed a race condition related to clearing expired session data.

* Fixed serious bug (deadlock) with checker script / logging.

* Modified the way `PKiller` kills processes to avoid a potential race condition.

* Fixed several breaking issues with the grouping mechanism related to the
  version 10 changes.

* Fixed a bug that prevented WHDW from loading students' scores.

* Fixed a bug that caused some `<showhide>` tags' buttons to affect other
  showhide elements on the page instead of themselves.

* Fixed an issue with error reporting in the `'openid_connect'` auth type.

* Fixed an issue with the ``'login' authentication type that caused false
  negatives when checking passwords from different browsers.

* Fixed an issue with broken loading of uploaded files from the database.

* Fixed an issue that caused an error message not to be shown when `cs_form` was
  not defined.

* Replaced a lingering instance of `xrange`.

* Questions with the `'checkoff'` question type now render as totally empty if
  the description and name are empty, rather than as a single colon.

#### SECURITY:

* `nohup.out` files are now ignored when generating the `.zip` files of the
  CAT-SOOP source.

# Version 10.3.0

#### CHANGED:

* Moved all logs to one central location to make backups easier.

# Version 10.2.2

#### REMOVED:

* Removed several calls to the deprecated `cs_print` from the default "main"
  page.

* Removed "Acknowledgments" section from the default "main" page.

#### FIXED:

* Removed an extraneous print statement from the default handler.

* Remove HTML formatting from course name in `source.zip` downloads.

# Version 10.2.1

#### CHANGED:

* Prevented the creation of extra directories when reading nonexistent logs.

#### REMOVED:

* Removed the defunct `checker_reporter.js` script.

#### FIXED:

* Fixed a number of issues with sessions.

* Fixed an issue with serializing Python sets for logging.

# Version 10.2.0

#### ADDED:

* Added more options to modify the styling of the "login required" box.

#### CHANGED:

* Submission IDs are no longer visible by default, though they can be made
  visible with `cs_show_submission_id`.

* Logs once again use SQLite instead of RethinkDB.

#### REMOVED:

* Removed RethinkDB completely.

#### FIXED:

* Updated the location of the default remote sandbox.

# Version 10.1.0

#### ADDED:

* `cs_user_info` and `cs_username` are now always defined, even on pages that don't
  require authentication

#### CHANGED:

* The built-in `print` function can now be used instead of `cs_print` to inject
  page content from within `<python>` tags.

#### DEPRECATED:

* `print` should now be used instead of `cs_print` to inject page content from
  within `<python>` tags.  `cs_print` will be removed in a future version.

#### FIXED:

* Fixed issue with clearing session data.

* Fixed an issue with QTYPEs' "init" functions being called with incorrect
  arguments.

# Version 10.0.1

#### CHANGED:

  * RethinkDB is now used to store session data, as opposed to the file-based
    storage used in the past.

  * Slight changes to the way version numbers are displayed in the default
    template.

#### FIXED:

  * Fixed multiple large issues with manually-graded submissions.

  * Fixed multiple large issues with the `'fileupload'` question type.

# Version 10.0.0

#### ADDED:

  * Added the dot product operator to expression question type.

  * Added `process.py`, containing some common operations relating to process
    management.

  * Added support for storing uploaded files on disk rather than in the
    database directly.

#### CHANGED:

  * RethinkDB is now a hard dependency.

  * The API for logging functions was changed, to avoid a special character as
    a separator.

  * Submissions are now handled asynchronously, and results are retrieved via
    web sockets.

  * Markdown no longer outputs `<em>` or `<strong>`, preferring `<i>` and `<b>` to
    improve browser compatibility.

  * Many images are now included in pages as data URI's rather than being
    loaded from a separate request.

  * Replaced the CSS-based loading spinner with an image.

  * Changed the password hashing scheme for the login authentication type to
    one based on <https://blogs.dropbox.com/tech/2016/09/how-dropbox-securely-stores-your-passwords/>

  * Re-styled buttons throughout the system.

#### REMOVED:

  * Removed CSS-only spinner, replaced with data URI for image in the base
    context.

  * Removed images for check/cross/favicon, replaced with data URI's in the
    base context.

  * Removed SQLite and catsoopdb as backends for logging.

#### FIXED:

  * Improved the Javascript code responsible for making sure the top menu
    doesn't block text when moving around a page.

  * Errors in the registration form now prevent submitting the form.

# Version 9.4.3

#### ADDED:

  * Added the `'circuit'` question type.

  * Added support for nonscalar values in `'expression'` questions.

#### CHANGED:

  * Changed exponentiation to be right-associative in the `'expression'` question
    type.

#### FIXED:

  * Fixed a bug with rendering implicit multiplication in `'expression'` questions.

  * Fixed a bug with rendering of chained exponentiation in `'expression'`
    questions.

  * Fixed a bug with order of operations when using the Python syntax in
    `'expression'` questions.

# Version 9.4.2

#### ADDED:

  * Added the `<section*>` family of tags, for unnumbered sections.

  * Added the `<include>` tag, to include the contents of another file.

  * Added the ability to add an additional message to the default page, via
    `cs_main_page_text`.

#### FIXED:

  * Fixed an issue whereby expressions utilizing binary subtraction were not
    properly parenthesized in the "Check Syntax" rendering of expression
    questions.

# Version 9.4.1

#### FIXED:

  * Fixed an issue with rendering of certain test cases in the `'pythoncode'`
    question type.

  * Modified the handling of streams in the Python sandbox to avoid buffers
    filling up.

  * Modified error message handling in the Python sandbox to avoid long-running
    regex searches.

# Version 9.4.0

#### ADDED:

  * Added support for custom "submissions not allowed" messages via
    `cs_nosubmit_message`.

  * Added the `cs_now` variable to page load contexts.

  * Added the `code_pre` option to tests in the pythoncode question type, for
    code to be run before submitted code.

#### CHANGED:

  * Code is now licensed under the GNU Affero General Public License, v3+.
    Footer text has been updated to reflect this change.

  * `cs_nsubmits_message` was replaced by `csq_nsubmits_message`.

  * Updated MathJax to version 2.7.1, and updated it to use a future-proof
    renderer.

  * Updated PLY to version 3.10.

  * Updated Markdown to version 2.6.8.

  * Updated BeautifulSoup to version 4.6.0.

  * Fixed several issues with rendering of `'pythoncode'` question types.

  * Updated the "Formatting Help" page of the `'richtext'` question type.

#### REMOVED:

  * Removed large pieces of the default MathJax install because they will not
    be used.

#### FIXED:

  * Fixed broken "home" link for pages in the `cs_util` "pseudo-course."

  * Fixed issue with HTML tags not rendering inside of `<section>` tags.

  * Fixed error messages for expressions that can't be parsed in the expression
    question type.

  * Fixed a crash when the `<cs_data_root>/courses` directory did not exist or
    was not writeable by the web server.

  * Fixed a bug with HTML tags being ignored inside `<ref>` tags.

# Version 9.3.0

#### ADDED:

  * Default handler now logs scores of all questions in the `'problemactions'` log
    on `'submit'` actions.

  * Added the `csq_always_show_tests` option to `'pythoncode'` questions, to
    enable/disable the "Show/Hide Detailed Results" button.

  * New plugin infrastructure.

  * `catsoop.ajaxrequest` now accepts a callback function that can be executed
    once the request has completed.

  * More options for rendering pages (`'content_only'` and `'raw_html'`).

#### CHANGED:

  * Code is now released under version 2 of the Soopycat License.

  * Math expressions are now rerendered in all responses to AJAX calls.

  * Updated KaTeX to version 0.7.0

  * Updated MathJax to version 2.7.0

  * Python syntax highlighting updated.

  * Several improvements to WHDW and stats displays.

#### REMOVED:

  * Errors related to evaluating the expression are no longer displayed in the
    `'pythonic'` question type.

#### FIXED:

  * Allowed `'pythonic'` question type to accept tuples without parentheses.

  * Fixed issue with `'expression'` question type, with `csq_ratio_check = True` and
    `csq_soln = 0`.

  * Switched to different CDN for loading Ace editor code.

  * Fixed an issue related to automatic locking when no answers have been
    viewed.

  * Fixed a regression related to rendering the answers to `'multiplechoice'`
    questions using the `'checkbox'` renderer.

  * Fixed a regression related to answer checking in `'multiplechoice'` questions.

  * Fixed issue related to handling empty `<python>` or `<question>` tags.

  * Fixed issue related to incorrectly filtering user information when
    generating API tokens.

  * Fixed issues with type errors from decoding data URI's in the `'pythoncode'`
    and `'fileupload'` question types.

  * Fixed several issues related to invalid HTML output in response to
    `'pythoncode'` submission.

  * Fixed `source.zip` generation to be more pedantic about when to re-build the file.

  * Fixed rendering of questions via `'rerender'` in the default handler.

  * Fixed problem with trying to render a template from a context where not all
    variables are defined.

  * Fixed rendering of check/cross images in expression question type.

  * Manual grading interface now displays more relevant feedback to the grader
    after submission (exactly the score and comments as the student will see
    them).

  * Fixed the display of answers to `'pythoncode'` questions so that syntax
    highlights properly.

  * Fixed issue with processes not being properly closed with the Python
    sandbox.

#### SECURITY:

  * Fixed a regression that opened a XSS vulnerability in `'pythoncode'` questions.

#### DOCUMENTATION:

  * Use American spelling in documentation.


# Version 9.2.0

#### ADDED:

  * Added `render_single_question` to the default handler, for rendering a single
    question's contents.

  * Added ability to customize how e-mail address is created from available
    information when using OpenID Connect.

  * Added the ability to override `get_group`'s section lookup and instead use
    the specified section.

  * Added `'stats'` and `'whdw'` (Who Has Done What) actions to the default handler.

#### CHANGED:

  * Better checks and display for `'checkoff'` question type.

  * Improved error handling in the `'pythonic'` question type.

  * Improved the formatting of HTML in the `'pythoncode'` question type to prevent
    Beautiful Soup from modifying it too much.

  * `None` is now a special category that is skipped when assigning groups.

  * Improved formatting of answers to `'pythoncode'` questions.

  * Cells in HTML tables are no longer automatically center-aligned.

  * Removed confusing answer display from `'checkoff'` question type.

#### FIXED:

  * Fixed a bug with `cs_path_info` not being defined in certain situations.

  * Fixed the stylesheet so that pages can be printed again.

  * Fixed a bug with reporting error messages related to malformed questions.

  * Fixed a type error in `list_groups` that prevented students' sections from
    being determined correctly.

  * Brought the `'richtext'` question type up to speed with current CAT-SOOP.

  * Brought the `'multiplechoice'` question type up to speed with current CAT-SOOP.

  * Brought the handout handler up to speed with current CAT-SOOP and improved
    its error reporting.

  * Fixed a regression that rendered `'pythonliteral'` questions unusable.

  * Fixed a typo in `dispatch` that broke proper 404 handling of handouts.

  * Fixed a regression with the permissions check for the "Save" button.

  * Several fixes for the `'fileupload'`, `'richtext'`, and `'pythoncode'` question types.

  * Prevented a misleading error message from being displayed when
    automatically viewing answers on a timed exercise.

  * Several fixes for `catsoop.tools.data_uri`

  * Fixed a bug that prevented per-user randomness from functioning as
    expected.

  * Updated automatic source downloader to use `spoof_early_load`, fixing a bug.

  * Fixed a typo in the `'checkoff'` question type.

# Version 9.1.0

#### ADDED:

  * Added `catsoop.path_info` added to javascript (for groups).

  * Added `cs_pre_handle` for normal use and `pre_handle.py` for plugins.

  * The state of the context is now stored after every `preload.py` file in the
    chain has been executed (in `cs_loader_states`), to allow, e.g., looking up
    parents' names.

  * Added "breadcrumbs" to the default theme.

  * Fleshed out the interactive group management page.

#### CHANGED:

  * Navigational menu entries can now optionally be specified in a more
    straightforward (Pythonic) manner.

  * `csq_names` are now automatically assigned as part of the XML parsing step,
    rather than in the default handler.

  * Changed "Are you sure you want to view the answer" dialog to use
    SweetAlert2 (<https://limonte.github.io/sweetalert2/>).

  * Nicer-looking tables in default theme.

  * Added a more complete message to the "log in" box for OpenID Connect.

#### FIXED:

  * Fixed bug with `cs_view_without_auth` flag.

  * Fixed bug with listing groups.

  * Fixed bug with file locking stemming from a change in 9.0.0.

  * `cs_post_load` and plugins' post-load hooks now fire at the right time (after
    `<python>` tags are evaluated).

  * Fixed bug with additional files in the Python sandbox.

  * Fixed several bugs related to HTML parsing/display.

  * Internal Server Errors in AJAX callbacks now display an error message.

  * Fixed a bug that prevented saving entries to questions.

  * Fixed a bug that prevented reading user information from the `__USERS__`
    directory from `catsoop.util.read_user_file`.

#### SECURITY:

  * Logins are no longer carried over between courses (user must log in to each
    course separately).

# Version 9.0.0

#### ADDED:

  * Added back the catsoopdb format (last seen in version 4.0.1), with
    improvements to prevent collisions and a few bugfixes.

  * Added Beautiful Soup to the distribution.
    (<https://www.crummy.com/software/BeautifulSoup/>)

  * Included Bootstrap (<http://getbootstrap.com/>) in the distribution, and
    updated the default theme to use it (also moved `main.template` and `base.css`
    to `old.template` and `old.css`, respectively, to make room for new style).

  * Added the `cs_base_color` variable, for switching the main color of the
    default theme.

  * Added syntax highlighting to code blocks, via highlight.js.
    (<https://highlightjs.org/>).  If no language is explicitly specified for
    syntax highlighting, the value stored in `cs_default_code_language` will be
    used (default is no syntax highlighting; explicitly setting that value to
    None will cause highlight.js to guess the appropriate language for each
    block).

  * Added support for authentication via OpenID Connect.
    (http://openid.net/connect/)

  * Added support for default courses via `cs_default_course`.

  * Added the `list_questions` and `get_state` API endpoints to the default
    handler.

  * Added an easier way to spoof context loading with `loader.spoof_early_load`.

  * Added `cslog.modify_most_recent`, which updates the most recent log entry
    atomically.

  * Added method for sending e-mail to a CAT-SOOP user from the API without
    knowing their e-mail address.

  * Added support for input checks and ratio checking (rather than absolute
    error checking) to the `'expression'` question type.

  * Added `'number'` question type for single numbers (or simple fractions).

  * Added `__PLUGINS__` directory for plugins which work in ways other than
    defining a new QTYPE or handler (ability to affect the context before or
    after preload, before or after content load, and after handler is invoked).

#### CHANGED:

  * CAT-SOOP is now only compatible with Python version 3.5+.  Python 2
    compatibility was dropped intentionally, but versions 3.0.0 <= x < 3.5 are
    not supported because CAT-SOOP does some strange things with imports.

  * CAT-SOOP no longer runs on Windows hosts.

  * Drastically improved inheritance for question types (requiring far less
    manual work) via `tutor.qtype_inherit`.

  * CAT-SOOP XML parsing is now largely handled by Beautiful Soup instead of by
    regular expressions.

  * Renamed `gb.py` -> `base_context.py` to more accurately reflect its usage.

  * Modified the login authentication type to use Python's built-in
    implementation of PBKDF2.

  * Changed the way authentication is handled in AJAX requests, in preparation
    for including the public-facing API.

  * Themes are now run through a pre-processor that handles `<python>` and
    `<printf>` tags (including `@{...}` syntax).

  * Passwords (in all forms) are now hashed both before and after being sent to
    the server (passwords are now never sent in plain-text).

  * Navigation links should now be held in `cs_top_menu` instead of
    `cs_navigation`.

  * `<ref>` tags can now take the relevant label as `label="x"` in addition to just
    as `x`.

  * Improved error pages shown on 404 File Not Found.

  * Default permissions now include the `'view'` permission.

  * Modified the default "loading" spinner to use CSS instead of an image

  * `csq_check_function` can now return a dictionary mapping `'score'` to the score
    and `'msg'` to a message to be returned, eliminating the need for
    csq_msg_function when it is more convenent to compute the score and message
    at the same time.  Alternatively, it can return a tuple `(score, message)`.
    The old form is still supported.

  * `csq_msg_function`, if used, can now optionally take a second argument
    representing the solution (the message doe not need to be computed from the
    submission alone).

  * Replaced `"response"` field with `"message"` in JSON returned by the default
    handler's AJAX calls, to avoid duplicate use of `"response"`.

  * The CAT-SOOP cat changes when displaying a 404 or 500 error message.

  * Improved error reporting in `'pythonic'` question type.

#### FIXED:

  * Tracebacks in CAT-SOOP error messages now actually show useful information.

  * Pre-compiled CAT-SOOP (.pycs) files' names now include the Python
    implementation's cache tag, so that the same course can be migrated to a
    CAT-SOOP instance running on a different version of Python without issue.

  * Fixed a bug whereby an empty entry in a `'multiplechoice'` question (--) was
    interpreted as being the last element in the csq_options list.

  * Missing files/directories are now always handled as 404 errors, rather than
    500.

  * Fixed a bug resulting from a nonexistent courses directory.

  * The `cs_post_load` hook is now invoked at a time when `cs_content `is still
    relevant.

  * Fixed bug with expression question type erroring when using multiple values
    for a variable.

  * Prevented PLY from writing its parsing tables to disk.

#### SECURITY:

  * Included the option to tune the number of iterations used with PBKDF2, and
    increased the default number of iterations from 50,000 to 250,000.

  * Minimum password length in the login authentication type is now 8 instead
    of 5 (per the NIST recommendation at <https://pages.nist.gov/800-63-3/sp800-63b.html>)


# Version 8.0.0

#### ADDED:

  * `<label>` and `<ref>` tags are now available, for easier referencing of
    sections within a CAT-SOOP page.

  * Answers and explanations can now be automatically viewed in certain
    situations (running out of submissions, earning 100% score).

  * Added a check for non-ASCII characters in input, and an error message to be
    displayed in this case.

  * Most CAT-SOOP options related to the default handler can now be specified
    as functions that return the appropriate value, rather than the value
    itself, which allows them to be set in a way that depends on the current
    context.

  * Added a way to compute stats about a particular page (for use in making
    gradebooks).

  * Question types can now have multiple form fields by having names starting
    with `__QNAME_`, where `QNAME` is the name of the question.

  * The `'multiplechoice'` question type has two new modes which allow for
    arbitrary formatting (including math) in the options: `'checkbox'`, which
    allows multiple answers to be selected; and `'radio'`, which allows only one
    answer to be selected.

  * Added the `cs_debug` function, which can be used to log arbitrary information
    to a file during execution of a preload or content file.

  * Resources can now be loaded from arbitrarily-named files (e.g.,
    `<root>/path/to/foo.md` instead of `<root>/path/to/foo/content.md`)

  * In the `'pythoncode'` question type, it is now possible to hide the code
    associated with test cases.

  * Added `data_uri` module from <https://gist.github.com/zacharyvoase/5538178>
    for better handling of file uploads

  * Users can now log in with their e-mail addresses instead of their usernames
    when using the `'login'` authentication type.

  * Permissions can now be specified directly via `cs_permissions`, instead of
    exclusively via roles.

  * The `'pythoncode'` question type can now handle Python 3 code.

  * Handlers and question types can now have viewable pages inside them,
    viewable at `<url_root>/__HANDLER__/default/page_name`

  * Every page footer now links to both the terms of the license, and also to
    the "download source" link.

  * Added a module for sending e-mails, primarily for use in the `'login'`
    authentication type.

  * [MathJax](https://www.mathjax.org/) is now included directly, rather than
    loaded from their CDN.

#### CHANGED:

  * Functions inside of question types no longer need to manually load default
    values; values from the defaults variable are automatically used when not
    specified inside the `<question>` tag.

  * The `'login'` authentication type was much improved, including the option to
    send confirmation e-mails, change passwords, and recover lost passwords;
    and to customize the types of e-mail addresses that are accepted.

  * Improved error reporting in the `'login'` auth type.

  * The `cs_post_load` hook now executes before the page's handler is invoked,
    and a new hook `cs_post_handle` was introduced, which is called after the
    handler is invoked.

  * CAT-SOOP's handling of HTML tags is now case-insensitive.

  * The "view as" page was updated to show more accurately what the user in
    question would see.

  * Many options related to the default handler (primarily related to which
    actions should be allowed) are now specified on a per-question basis rather
    than a per-page basis.

  * Locking a user out of a problem has been separated from viewing the answer
    to that question.

  * Improved rendering in the `'expression'` question type.

  * `name_map` is now stored as an ordered dictionary.

  * Results from the `'pythonic'` question type are now evaluated in the question's
    scope, rather than in the question type's scope.

  * The number of rows to be displayed in the ACE interface for coding
    questions is now customizable.

  * Answers in the `'smallbox'` and `'bigbox'` question types are no longer wrapped in
    `<tt></tt>`.

  * Markdown and/or custom XML, depending on the source type used, is now
    interpreted inside of answers and explanations (including math rendering).

  * All CAT-SOOP modules are now available inside of the source files for
    handlers and question types.

  * The `cs_scripts` string is now injected into the template after jQuery,
    katex, MathJax, and cs_math have been loaded.

  * Modified the generation of per-user random seeds to (eventually) allow for
    re-generating of random seeds.

  * Moved much of the Javascript code from the default handler to separate
    files.

  * Moved WSGI file and changed the way imports are handled in order to make
    sure everything can access the CAT-SOOP modules/subpackages.

  * Moved handling of `csq_prompt` out of individual question types and into the
    default handler to avoid duplicating code.

  * Removed logo image from main page.

  * `cs_source_format` is now inferred (rather than specified explicitly).

  * In question type specifications, `handle_submission` now returns a dictionary
    instead of a tuple.

  * Restructured authentication types to make adding more types in the future
    easier.

  * Section labels are now rendered as id's of the associated headers.

#### FIXED:

  * Fixed a bug whereby `$` characters could not be escaped with backslash.

  * Fixed issues with certain tags' internals being parsed as Markdown (`script`,
    `pre`, `question`, etc).

  * Trying to access a resource that doesn't exist on disk now gives a 404
    error instead of crashing.

  * Fixed several bugs related to uploading multiple files in a single
    submission

  * Spaces are now allowed in question names.

  * CAT-SOOP no longer crashes on a malformed `<question>`, but rather displays
    an error message.

  * Fixed an issue with intermittent WSGI failures by re-trying failed actions.

  * Updated MathJax to version 2.6.1 to fix a rendering issue in Chrome.

  * Updated the URL of the default Python sandbox to reflect changes in the
    CAT-SOOP web site.

  * Improved handling of query strings and fragment identifiers when rewriting
    URLs.

  * Improved handling of implicit multiplication in the `'expression'` question
    type.

  * Added unary `+` to Python syntax in the `'expression'` question type.

  * `cslog.most_recent` now returns the default value when the log file does not
    exist, instead of crashing.

  * Fixed handling of temporary files on Windows hosts.

  * Fixed validation of user information when registering under the `'login'`
    authenatication type.

  * Fixed several bugs with manual grading, reported from 6.02.

  * Log files are no longer created when trying to read from a nonexistent log.

  * Mercurial temporary files (`.orig`) are now ignored in the zip generated when
    downloading the source.

  * `<pre>` tags are now used instead of `<tt>` for wrapping answers in the
    `'pythoncode'` question type.

  * Fixed an issue in the `'pythoncode'` sanboxes whereby 0 MEMORY limit actually
    allowed 0 bytes of heap storage, rather than unlimited.

  * Prevent a crash if `<cs_data_root>/courses` does not exist.

  * Modified to always use the local `markdown` package, even if one is installed
    globally, to make sure Markdown extensions are loaded properly.

  * Buttons are now re-enabled on page load, to prevent an issue whereby
    buttons would remain disabled after a refresh on Firefox.

#### SECURITY:

  * PBKDF2 (https://en.wikipedia.org/wiki/PBKDF2) is now used for the `'login'`
    authentication mode.

  * Closed a XSS vulnerability in the `'pythoncode'` question type.

  * Closed a security hole in session handling that allowed for arbitrary code
    execution under certain circumstances by validating session ids and
    modifying the way session data are stored.

  * Logs can no longer be accessed/created outside of the appropriate `__LOGS__`
    directories.

# Version 7.1.1

#### FIXED:

  * Fixed an issue that prevented the last question on each page from being
    displayed.


# Version 7.1.0

#### ADDED:

  * Added the option to grade questions manually, from 6.02 fall 2015.

  * Added a `'richtext'` question type, which allows for formatting of text using
    CAT-SOOP-flavored Markdown.

  * Added the `'fileupload'` question type, which allows users to upload arbitrary
    files.

  * Added checks for valid configuration options.

#### CHANGED:

  * Rewrote the `'expression'` question type to use PLY for parsing, and included a
    default syntax for expressions that is more approachable to users not
    familiar with Python.

# Version 7.0.1

#### FIXED:

  * Fixed a syntax error in the expression question type.


# Version 7.0.0

ADDED:

  * Included KaTeX (https://khan.github.io/KaTeX/).

  * Added three new handlers: `'passthrough'`, which displays `cs_content` without
    modification; `'raw_response'`, which allows sending a raw HTTP response; and
    `'redirect'`, for redirecting to other resources easily.

  * Added support for [Markdown](https://daringfireball.net/projects/markdown/)
    as an alternative source format, and included [`python-markdown`](https://pypi.python.org/pypi/Markdown)
    in the distribution.

  * Question type specifications can now include an arbitrary action (beyond
    saving/submitting) that will be executed when a user presses a new button.

  * Added support for streaming content (via returning a generator instead of a
    string), and for automatic streaming of large static files.

  * Added support for inline (runnable by users) test cases in `'pythoncode'`
    question types.

  * Added `cs_util` resources: `time`, which yields the current time (according to
    the server) for synchronization purposes; `source.zip`, which downloads a zip
    archive containing the CAT-SOOP source code; and `license`, which contains
    the text of CAT-SOOP's license.

  * Added a `'string'` mode to the `'pythonic'` question type, which allows the answer
    to be specified as a string to be evaluated.  Also added the `csq_code_pre`
    variable to this question type, for setting up the environment into which
    `csq_soln` will be evaluated in string mode.

#### CHANGED:

  * Math rendering now uses KaTeX (fast, but limited support) when possible,
    and falls back to MathJax (slow, but more support) when necessary.

  * "Special" CAT-SOOP variables are now prefixed with `cs_` (for page-specific
    values) or `csq_` (for question-specific values) to prevent accidental
    shadowing

  * Changed nomenclature: "activity type" -> "handler"

  * Complete rewrite of default handler.

  * Reorganization of sandboxing for Python code.

  * `gb.py` should no longer be changed; rather, global configuration values
    should be overwritten via `config.py` (which is loaded into `gb.py`)

  * Improved handling of footnotes.

#### REMOVED:

  * Removed `jquery_typing` plugin, which is no longer needed for expression
    questions.

#### FIXED:

  * Fixed bug with newline handling in CGI interface.

  * Fixed bugs related to static files when using the CGI interface running on
    Windows hosts.

  * The default theme now handles resizing of the containing window more
    smoothly.

#### STYLE:

  * Mostly embraced PEP8 style (https://www.python.org/dev/peps/pep-0008/).


# Version 6.0.0

#### ADDED:

  * Added `post_load` hook, which is executed after the content file is executed.

  * Added support for XML as an additional source format, and set it to be the
    default format.

  * Change names `EARLY_LOAD.py` -> `preload.py` and `LATE_LOAD.py` -> `content.xml`.

  * Added the `'pythonliteral'` question type, which behaves much like pythonic,
    but requires that the submission be a literal value (rather than the result
    of a more complicated expression).

#### CHANGED:

  * Modified handling of footnotes.

  * File containing user information should now end in `.py` (e.g., `username.py`
    instead of just `username`).

  * Reorganized `'python...'` question types to properly inherit from one another
    to avoid duplicate code.

#### REMOVED:

  * The `'problem'` activity type was removed, in favor of `'ajaxproblem'`.

#### FIXED:

  * Fixed an issue where `'last_submit'` was keeping information only about the
    most recent submission overall, instead of the most recent submission for
    each question.

  * The `__LOGS__` directory will now be created if it does not exist, rather
    than crashing CAT-SOOP.

#### SECURITY:

  * Error messages now show less information, to avoid displaying sensitive
    information.

# Version 5.0.0

ADDED:

  * Added support for footnotes via `<footnote>`

  * Added support for page organization via `<section>`, `<subsection>`, etc.

  * The ability to save and submit are now controllable via special variables
    in the problem activity type.

  * Added a warning message upon clicking the 'view solution' button to
    indicate that users will not be able to submit after doing so.  Also
    maintained the ability to bypass this check, for things like automatically
    submitting at the end of a timed exercise.

  * Added the handout activity type, which allows for showing a static file,
    but with access controls (releasing after a particular date, only viewable
    by particular role, etc).

  * Added support for displaying explanations in addition to answers in
    particular question types.

#### CHANGED:

  * Logs are now stored in [SQLite](https://www.sqlite.org/) databases.

  * The logo in the main page is now displayed as text, rather than as an
    image.

  * Buttons in `'ajaxproblem'` question types are now disabled before processing
    the request, to avoid multiple identical submissions from mis-clicks.

#### REMOVED:

  * Removed catsoopdb format, in favor of SQLite.

#### FIXED:

  * Renamed `logging.py` to `cslog.py` to prevent accidentically importing Python's
    built-in `logging` module.

  * Fixed rendering of math when viewing solution to an `'expression'` question.

  * Scores are now properly handled in the `'ajaxproblem'` activity type.

  * Fixed a bug with displaying the solution for `'pythonic'` question types whose
    solutions are tuples.

  * Fixed a bug with displaying the solution for `'pythonic'` question types whose
    solutions are strings.

  * Fixed a bug related to handling of dynamic pages in the `__BASE__` course.

  * Fixed numerous `'ajaxproblem'` bugs.

  * Improved detection of static files.

#### SECURITY:

  * Error messages no longer show information about the location of CAT-SOOP
    (or the course in question) on disk

# Version 4.0.1

#### FIXED:

  * Fixed issue whereby a missing `EARLY_LOAD.py` would crash CAT-SOOP.

  * Fixed bug with caching of static files.

  * Fixed bug related to authenticating in `'login'` mode.

#### REMOVED:

  * Removed rendering time from default template.

# Version 4.0.0

#### ADDED:

  * Added the `'ajaxproblem'` activity type, which allows submitting individual
    questions without reloading the entire page.  Made `'ajaxproblem'` the default
    activity type.

  * Added support for skipping ahead or behind by weeks in relative
    timestrings, using `+` or `-` (e.g., `M+:17:00` means _next_ Monday at 5pm).

  * Solutions for individual students are now displayed when impersonating
    them.

  * Source for pages is now cached in a marshaled format, to prevent having to
    re-parse the source of pages that have not changed.

  * Added support for authenticating via login (username and password) rather
    than via client certificate.

  * Added support for per-user randomness (users see the same numbers upon
    returning to a page, but different users may see different numbers).

  * Added documentation (via epydoc-compatible docstrings) throughout.

  * CAT-SOOP now asks the browser to use cached versions of static files where
    appropriate.

  * Allowed question types and activity types to be specified in the course
    rather than in the base system.

#### CHANGED:

  * Changed internal nomenclature: meta -> context everywhere to represent the
    context in which a page is rendered.

#### REMOVED:

  * Removed several references to `sicp-s2.mit.edu` in the code.

#### FIXED:

  * Fixed impersonation glitch whereby permissions were inherited from the
    impersonated user.

  * Fixed glaring bug with static file handling.

  * Fixed inheritance bug in the pythonic question type.

# Version 3.1.0

#### ADDED:

  * Added WSGI interface (and moved main function elsewhere so WSGI and CGI can
    share code).

  * Questions are automatically given names if they were not explicitly given a
    name.

  * Question types and activity types are now pre-compiled to avoid having to
    re-parse them on every load.

# Version 3.0.0

#### ADDED:

  * problem activities now store due dates, to account for changes in due date
    after submitting.

  * Added support for the [ACE code editor](https://ace.c9.io/#nav=about) in
    Python code questions.

#### CHANGED:

  * Separated loading from `METADATA.py` into `EARLY_LOAD.py` and `LATE_LOAD.py`.
    ``EARLY_LOAD files are executed all the way down the source tree (for the
    sake of inheritance, as with `METADATA.py`), but only the `LATE_LOAD.py`
    associated with the leaf node is executed (to allow some code execution to
    be avoided when working down the tree).

  * Moved/improved impersonation code.

  * Refactored logging code.

  * Refactored main control loop.

#### FIXED:

  * Better sandboxing of Python code.

  * Fixed an issue with `submitAs` control for questions with randomness.

  * Fixed handling of paths on Windows hosts.

  * Modified expression question type to be compatible with Python 2.6.

  * Several bug fixes in `'pythoncode'` question type.

#### SECURITY:

  * Prune out `..` and `.` from URLs to avoid escaping the CAT-SOOP tree.

# Version 2.0.0

Complete re-write.  First version used in 6.01 (spring 2013).  First version
with any similarity to the current code.


# Version 1.0.0

The original version, used in 6.003 fall 2011, and described in
<http://dspace.mit.edu/handle/1721.1/77086>.  This version has _very little_ in common with
later versions.
