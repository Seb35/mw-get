mw-get
======

mw-get aims at automating and simplifying the deployments of [MediaWiki]
(https://www.mediawiki.org/).

It mainly targets the maintainers of small MediaWiki installations, to help
them to install and upgrade MediaWiki and its extensions.

Features
--------

* Install and upgrade MediaWiki installations and its extensions
* Can be configured to automatically install security releases
* Can track special branches: LTS and latest branch, beta and alpha releases
* Optional backup before upgrades [not-tested]
* Optional deployment in a preproduction directory and environment [not-tested]
* Download from tarballs or git repository

Interface
---------

Go to the directory where is/will be MediaWiki.

Some actions are not yet available. Downloads with git is always implemented,
but there are lacks with tarballs (extensions and upgrades).

It is assumed the script `mw-get` is located in `/usr/local/bin` or is
accessible in the path.

### Main actions

1. Install newest release of MediaWiki
   (Current directory must be empty.)
   
     mw-get install

2. Install newest release of MediaWiki
   (Current directory must be empty.)
   
     mw-get install --git

3. Install MediaWiki extensions
   (Extensions must be in the official MediaWiki Git repository.)
   
     mw-get install ContactPage BetaFeatures --git

4. Upgrade MediaWiki to the newest version in the same branch (minor upgrades)
   
     mw-get upgrade

5. Upgrade MediaWiki and extensions to their newest versions (major upgrade)
   
     mw-get major-upgrade 1.24

5. Get current version and branch
   
     mw-get version
     mw-get branch

### Supplementary actions

5. Install MediaWiki 1.25 or LTS release or beta (release candidate) or alpha
   
     mw-get install 1.25
     mw-get install lts
     mw-get install beta
     mw-get install alpha

6. Upgrade MediaWiki and extensions to a newer branch (major upgrade)
   
     mw-get major-upgrade 1.25
     mw-get major-upgrade lts
     mw-get major-upgrade beta
     mw-get major-upgrade alpha

### Main parameters

- --git[=/srv/git/mediawiki]
  
  Download with Git instead of tarballs.
  When an optional path or URL is given, it will be used as a source for git
  (with specific subdirectories appended: /core.git,
  /extensions/Interwiki.git, etc.)

- --user=Seb35
  
  Download with Git and use Gerrit account; useful for developers

- --backup=/var/lib/backups/mediawiki-%Y%m%d-%H%M%S.tar.gz
  
  Backup database before upgrading

- --preprod=/srv/www/mywiki-preprod
  
  Copy current MediaWiki directory into another preproduction directory and
  upgrade MediaWiki. In future version a hook will be optionally triggered to
  activate the virtual host, the DNS configuration, etc.

### Advanced parameters

- --git-options="--reference /srv/git/mediawiki/core.git"
  
  Pass libre options to `git clone`, e.g. if you have local objects available,
  etc.

- --dev
  
  Install developer dependencies

If MediaWiki is installed with Git and some of these parameters are specified,
they will be saved as Git config (mediawiki.*).


Version management
------------------

General case:
* Branch: the user should only specify the branch (e.g. 1.19) but not a
  specific release (e.g. 1.19.24)
* Security: by default upgrades install the last available security release
  in the branch
* Major upgrades: to upgrade to another branch, it is needed to explicitely
  specify it
* Extensions: extensions versions try to stay consistent with MediaWiki
  branches

Specific cases:
* LTS releases: this is just an alias for the newest LTS branch
* Latest releases: this is just an alias for the newest stable branch
  (LTS or not)
* Beta releases: when a beta release of a specific branch is installed, new
  upgrades go to newest beta releases then stable releases in the same branch
* Alpha releases: when an alpha release is used, new upgrades are always alpha
  releases and will never go to specific branches (if not specified)

Current and future development
------------------------------

This script is intended to cover most needs of the casual MediaWiki sysadmin,
so it should not become overly complex to adapt to very specific needs.
Additionally it should work on as many UNIX systems as possible (whence the
choice of shell script with common commands); Bash is currently used but it
would be better to downgrade to Bourne Shell if possible.

A basic test suite is available in `tests/test-mw-get`.

Planned features:
* Make more actions with tarballs as a source work (instalation of extensions,
  upgrades)
* Merge action `major-upgrade` into `upgrade 1.XX`
* Think about per-extension versions, e.g. `Vector=e19b0a4 Interwiki=8e300c1`
* Think about a reasonnable scenario for deployment with preproduction
* Test backup feature
* Think about an optional action `backup` (alone)
* In git, rely on remote branches as authoritative for upgrades, instead of
  local branches which can be modified (e.g. upgrade by rebasing origin/REL1_25
  instead of REL1_25), in order to make possible the use of local deployment
  branches (e.g. mysite/REL1_25).
* Internationalisation
* Improve verbosity and logging, currently everything is redirected to stderr

Quality:
* Better balance documentation between tutorial and complete doc
* More documentation, whose code style, dedicated interface doc, tutorial
* More tests
* More comments
* Try to better mock downloads of tarballs to avoid real downloads
  (add a cache)

Contact
-------

If you have an though or idea or usability issue with this script, please
[open an issue](https://github.com/Seb35/mw-get/issues) on Github. This script
aims at being useful, so any feedback is really appreciated!

* Licence: WTFPL 2.0
* Author: [Seb35](http://www.mediawiki.org/wiki/User:Seb35)

