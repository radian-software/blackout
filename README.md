**Blackout:** the easy way to clean up your Emacs mode lighters.

## Summary

Blackout is a package which allows you to hide or customize the
display of major and minor modes in the mode line.

## Installation

Blackout is available on [MELPA][melpa]. My favorite option is to
install it using [`straight.el`][straight.el]:

    (straight-use-package 'el-patch)

However, you may install using any other package manager if you
prefer, including the built-in `package.el`, assuming you configure it
to talk to MELPA.

## Usage

Hide a minor mode:

    (blackout 'auto-fill-mode)

Change the display of a minor mode (note the leading space; this is
needed if you want there to be a space between this mode lighter and
the previous one):

    (blackout 'auto-fill-mode " Auto-Fill")

Change the display of a major mode:

    (blackout 'emacs-lisp-mode "Elisp")

Operate on a mode which hasn't yet been loaded:

    (with-eval-after-load 'ivy
      (blackout 'ivy-mode))

## `use-package` integration

Any of these:

    (use-package foo
      :blackout)

    (use-package foo
      :blackout t)

    (use-package foo-mode
      :blackout t)

    (use-package bar
      :blackout foo-mode)

Results in:

    (blackout 'foo-mode)

Each of these:

    (use-package foo
      :blackout " Foo")

    (use-package bar
      :blackout (foo-mode . " Foo"))

Results in:

    (blackout 'foo-mode " Foo")

This:

    (use-package quux
      :blackout ((foo-mode . " Foo")
                 (bar-mode . " Bar")))

Results in:

    (blackout 'foo-mode " Foo")
    (blackout 'bar-mode " Bar")

## Advanced notes

Some minor modes have a variable name that is different from the mode
name. For example, the variable name for `auto-fill-mode` is actually
`auto-fill-function`. In this case, you should provide the variable
name rather than the mode name. However, since this is unintuitive,
Blackout tries to automatically fix it for you using the information
in the user option `blackout-minor-mode-variables`; this is why you
can just write `(blackout 'auto-fill-mode)` and it works.

## Comparison

There are at least three other popular packages similar to Blackout:

* [`diminish.el`][diminish]
* [`delight.el`][delight]
* [`dim.el`][dim]

Blackout provides the same core features as all three of these
packages, and is simpler to use. The following section shows
comparisons of the same code in all four packages.

    ;; Standard pre-loaded minor mode with a non-matching variable name
    (blackout 'auto-fill-mode)
    (diminish 'auto-fill-function)
    (delight 'auto-fill-function nil t)
    (dim-minor-name 'auto-fill-function nil)

    (blackout 'auto-fill-mode " Auto-Fill")
    (diminish 'auto-fill-function "Auto-Fill")
    (delight 'auto-fill-function " Auto-Fill" t)
    (dim-minor-name 'auto-fill-function " Auto-Fill")

    ;; Major mode
    (blackout 'emacs-lisp-mode "Elisp")
    ;; not supported by diminish.el
    (delight 'emacs-lisp-mode "Elisp" :major)
    (dim-major-name 'emacs-lisp-mode "Elisp")

    ;; Minor mode, not pre-loaded
    (with-eval-after-load 'ivy
      (blackout 'ivy-mode))
    (with-eval-after-load 'ivy
      (diminish 'ivy-mode))
    (delight 'ivy-mode nil 'ivy)
    (dim-minor-name 'ivy-mode nil 'ivy)

    ;; foo-mode provided by foo.el
    (use-package foo
      :blackout)
    (use-package foo
      :diminish)
    (use-package foo
      :delight)
    ;; not supported by dim.el

    (use-package foo
      :blackout " Foo")
    (use-package foo
      :diminish "Foo")
    (use-package foo
      :delight " Foo")
    ;; not supported by dim.el

    ;; foo-mode and bar-mode provided by quux.el
    (use-package quux
      :blackout ((foo-mode . " Foo")
                 (bar-mode . " Bar")))
    (use-package quux
      :diminish ((foo-mode . "Foo")
                 (bar-mode . "Bar")))
    (use-package quux
      :delight (foo-mode " Foo")
               (bar-mode " Bar"))
    ;; not supported by dim.el

## Contributor guide

Please see [the contributor guide for my
projects](https://github.com/raxod502/contributor-guide) for the most
part. However, this project is small enough that I haven't bothered to
set up linters, so you don't need to worry about that.

[delight]: https://elpa.gnu.org/packages/delight.html
[dim]: https://github.com/alezost/dim.el
[diminish]: https://github.com/myrjola/diminish.el
[melpa]: http://melpa.org
[straight.el]: https://github.com/raxod502/straight.el
[use-package]: https://github.com/jwiegley/use-package
