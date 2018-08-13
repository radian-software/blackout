**Blackout:** the easy way to clean up your Emacs mode lighters.

## Summary

Blackout is a package which allows you to hide or customize the
display of major and minor modes in the mode line.

## Installation

The easiest way to install Blackout is using
[`straight.el`][straight.el]:

    (straight-use-package
     '(blackout :host github :repo "raxod502/blackout"))

You may use any other package manager which supports installation from
source (Blackout is not yet listed on MELPA).

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

## Comparison

There are at least three other popular packages similar to Blackout:

* [`diminish.el`][diminish]
* [`delight.el`][delight]
* [`dim.el`][dim]

Blackout provides the same core features as all three of these
packages, and is simpler to use. The following sections show
comparisons of the same code in all four packages.

    (blackout 'auto-fill-mode)
    (diminish 'auto-fill-function)
    (delight 'auto-fill-function)
    (dim-minor-name 'auto-fill-function nil)

    (blackout 'auto-fill-mode " Auto-Fill")
    (diminish 'auto-fill-function "Auto-Fill")
    (delight 'auto-fill-function " Auto-Fill" "simple")
    (dim-minor-name 'auto-fill-function " Auto-Fill")

    (blackout 'emacs-lisp-mode "Elisp")
    ;; not supported by diminish.el
    (delight 'emacs-lisp-mode "Elisp" :major)
    (dim-major-name 'emacs-lisp-mode "Elisp")

    (with-eval-after-load 'ivy
      (blackout 'ivy-mode))
    (with-eval-after-load 'ivy
      (diminish 'ivy-mode))
    (delight 'ivy-mode nil "ivy")
    (dim-minor-name 'ivy-mode nil 'ivy)

    (use-package foo
      :blackout t)
    (use-package foo
      :diminish t)
    (use-package foo
      :delight (foo-mode nil "foo"))
    ;; not supported by dim.el

    (use-package foo
      :blackout " Foo")
    (use-package foo
      :diminish "Foo")
    (use-package foo
      :delight (" Foo" nil "foo"))
    ;; not supported by dim.el

    (use-package quux
      :blackout ((foo-mode . " Foo")
                 (bar-mode . " Bar")))
    (use-package quux
      :diminish ((foo-mode . "Foo")
                 (bar-mode . "Bar")))
    (use-package quux
      :delight
      (foo-mode " Foo" nil "quux")
      (bar-mode " Bar" nil "quux"))
    ;; not supported by dim.el

## Advanced notes

Some minor modes have a variable name that is different from the mode
name. For example, the variable name for `auto-fill-mode` is actually
`auto-fill-function`. In this case, you should provide the variable
name rather than the mode name. However, since this is unintuitive,
Blackout tries to automatically fix it for you using the information
in the user option `blackout-minor-mode-variables`; this is why you
can just write `(blackout 'auto-fill-mode)` and it works.

[delight]: https://elpa.gnu.org/packages/delight.html
[dim]: https://github.com/alezost/dim.el
[diminish]: https://github.com/myrjola/diminish.el
[straight.el]: https://github.com/raxod502/straight.el
[use-package]: https://github.com/jwiegley/use-package
