
# MacroBase

This project is just a bunch of shell scripts that aid you creating macros in
Linux using the `xmacro` package.

In order to understand this little tool, you first need to understand what
macros are and how they can be made using `xmacro`. To do so, Google about it.

## Installation

It's recommended that you clone this repository for every work you have to do.
Otherwise, you will have to remove recorded macros every time you want to
automate another job.

First, clone this repo.

    git clone https://github.com/nerde/macrobase.git my_macro_job
    cd my_macro_job

You may remove the version control folder, if you will.

    rm -rf .git

If you don't have `xmacro` installed yet, you can install it by running:

    util/setup

## Scripts that help you

Some shell scripts were built to help you creating macros. So, in the cloned
repository, you can:

__Record a new macro__

    ./rec open_web_form

__Play a recorded macro__

    ./play 000_open_web_form

__Register a delay between macros__. It registers a flag that will make the
execution sleep for 5 seconds. The 'sleep' command will be used.

    ./delay 5

__Run all of it at once__

    ./run

## How it works

It has three main folders: `before`, `execution` and `after`. Each of them can
contain as many macros and delay flags as needed. You just need to create your
macros and delays using the `./rec` and the `./delay` commands, for example:

    ./rec open_my_website # And then you record this macro
    ./delay 3 # Wait 3 seconds so the website can be loaded
    ./rec fill_form # And now you record the form filling
    ./rec save_form # And record a click in the "Save" button

All of the steps will be registered so, when you are ready, you can call:

    ./run

The script will execute first and once what's in the `before` directory. After
that, it will run what's in the `execution` folder as many times as you asked
for. You can specify how many times you want it to run with a parameter, as you
can see below. And at last, it will run what's in the `after` directory.

    ./run -r 5 # Runs what's in the 'execution' folder five times.

By default, every time you record a macro using the `rec` script or register
a delay using the `delay` script, the result will be tossed in the `execution`
folder. If you want them to work with the `before` or `after` folders, you
need to call them like this:

    ./rec open_browser before
    ./delay 10 before # Waiting the browser to open.
    ./rec fill_form # When no folder is given, the 'execution' will be the one.
    ./rec close_browser after

### The order

Every time you record a macro or register a delay, it will be created like this:

    001_foo.mac
    002_bar.mac
    003_delay_5.mac

This is done this way because the order the files are listed (alphabetical) is
the order that they will get run. So, it's recommended that you record the
macros and the delays in the order you need them to run, the scripts will take
care of the ordering for you.

## Debugging

If some macro yolu recorded is messing up with something, you can debug it by passing
the `-g` argument. This will make the script tell you what file it's going to
run before doing it so you can easily find where is the problem.

    ./run -g

## The delay problem

If you know the `xmacro` package, you may know that, in a macro file,
you can type `Delay 5` and `xmacro` will delay 5 seconds for you. However, some
time ago when I used this solution, I realized that not every time `xmacro` was
respecting my Delay command. Because of this, it was needed to run the delays
via shell script and that's why I created this feature here.
