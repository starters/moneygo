# MoneyGo

**MoneyGo** is a personal finance web application written in JavaScript and
Golang. It adheres to [double-entry
accounting](https://en.wikipedia.org/wiki/Double-entry_bookkeeping_system)
principles and allows for importing directly from financial institutions using
OFX (via [ofxgo](https://github.com/aclindsa/ofxgo)).

This project is in active development and is not yet ready to be relied upon as
your primary accounting software (but please feel free to try it out and offer
feedback!).

## Features

* [Import from OFX](./docs/ofx_imports.md) and
  [Gnucash](http://www.gnucash.org/)
* Enter transactions manually using the register, double-entry accounting is
  enforced
* Generate [custom charts in Lua](./docs/lua_reports.md)

## Screenshots

![Yearly Expense Report](./screenshots/yearly_expenses.png)
![Transaction Register](./screenshots/transaction_register.png)
![Transaction Editing](./screenshots/editing_transaction.png)

## Usage Documentation

Though I believe much of the interface is 'discoverable', I'm working on
documentation for those things that may not be so obvious to use: creating
custom reports, importing transactions, etc. For the moment, the easiest way to
view that documentation is to [browse it on github](./docs/index.md).

## Installation

First, install npm, nodejs >= 6.11.3 (may work on older 6.x.x releases, but this
is untested), python, curl, and go >= 1.9 in your distribution. Here is how in
Arch Linux:

	sudo pacman -S npm curl go python

Install browserify globally using npm:

	sudo npm install -g browserify

You'll then want to build everything (the Golang and Javascript portions) using
something like:

	export GOPATH=`pwd`
	go get -d github.com/aclindsa/moneygo
	go generate -v github.com/aclindsa/moneygo/internal/handlers
	go generate -v github.com/aclindsa/moneygo
	go install -v github.com/aclindsa/moneygo

This may take quite a while the first time you build the project since it is
auto-generating a list of currencies and securities by querying multiple
websites and services. To avoid this step, you can `touch
src/github.com/aclindsa/moneygo/internal/handlers/cusip_list.csv` before
executing the `go generate ...` command above. Note that this will mean that no
security templates are available to easily populate securities in your
installation. If you would like to later generate these, simply remove the
cusip_list.csv file and re-run the `go generate ...` command.

## Running

MoneyGo requires HTTPS or FCGI (no HTTP). Before starting the server, you will
want to edit the example configuration file
(src/github.com/aclindsa/moneygo/internal/config/example_config.ini) to point to
your own SSL certificate/key OR set 'generate-certs-if-absent = true' in the
'[http]' section of the config file.

Then, assuming you're in the same directory you ran the above installation
commands from, running MoneyGo is as easy as:

	./bin/moneygo -config src/github.com/aclindsa/moneygo/internal/config/example_config.ini

You should then be able to explore MoneyGo by visiting https://localhost:8443 in
your browser. Editing the configuration file supplied will allow you to modify
several settings including the port used, SSL certificate locations, and whether
to serve via FastCGI instead of HTTPS (the default).

## Missing Features

* Importing a few of the more exotic investment transactions via OFX
* Budgets
* Scheduled transactions
* Matching duplicate transactions
* Tracking exchange rates, security prices
* Import QIF
