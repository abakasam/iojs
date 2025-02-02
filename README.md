# Google IO.js

Tool for runtime execution of node applications using Javascript.

## Running

This repository is PRECOMPILED.`

	./iojs --version

## Naming

	io.js		Software
	iojs.org	Website

## Build

### Environment Variables (CentOS 7.2.1511)

	GNU GCC				PATH > /usr/bin/gcc
	PYTHON				PATH > /usr/bin/python
	CLANG				?
	GNU Make			PATH > /usr/bin/make
	libexecinfo (BSD)	?

### Install Python

	cd /path/to/python
	export PYTHON=/path/to/python
	$PYTHON ./configure
	make

### Install io.js

	tar -xvf iojs-v3.3.1.tar.gz
	cd /path/to/iojs
	./configure
	make	
	make check
	make test
	make doc
	man doc/iojs.1
	iojs -e "console.log('Hello from io.js ' + process.version)"

## Contents

### Markdowns

- AUTHORS
- CHANGELOG
- COLLABORATOR_GUIDE
- CONTRIBUTING
- GOVERNANCE
- LICENSE
- ROADMAP
- README.md
- WORKING_GROUPS

### Directory

	.			io.js Source
	benchmark	?
	deps		?
	deps/npm	Node Package Manager
	dist		?
	doc			Documentation
	doc/api		API Documentation
	lib			Library
	out			?
	src			Source Code
	test		Source Code Test
	tools		?

### Files

	Releases: 		dist/
	Source:			iojs-v3.3.1.tar.gz
	Documentation
		API			all.html, doc/api
		Command		make doc; man doc/iojs.1
		io.js		?

## Download Verification


### Checksum

To check that a downloaded file matches the checksum, run it through `sha256sum` with a command such as:

```
$ grep iojs-vx.y.z.tar.gz SHASUMS256.txt | sha256sum -c -
```

### GPG keys

To verify a SHASUM256.txt.asc, import all GPG keys of individuals authorized to create releases. GPG keys are listed at [Release Team](#release-team).

Use a command such as this to import the keys:

```
$ gpg --keyserver pool.sks-keyservers.net \
  --recv-keys DD8F2338BAE7501E3DD5AC78C273792F7D83545D
```

## Configuration

### Android

Be sure you have downloaded and extracted [Android NDK]
(https://developer.android.com/tools/sdk/ndk/index.html)
before in a folder. Then run:

```
$ ./android-configure /path/to/your/android-ndk
$ make
```

### 'Intl' (ECMA-402)

[Intl](https://github.com/joyent/node/wiki/Intl) support is not
enabled by default.

### 'small' (English only) 

This option will build with "small" (English only) support, but
the full `Intl` (ECMA-402) APIs.  With `--download=all` it will
download the ICU library as needed.

Unix / Macintosh:

```text
$ ./configure --with-intl=small-icu --download=all
```

Windows:

```text
> vcbuild small-icu download-all
```

The `small-icu` mode builds with English-only data. You can add full
data at runtime.

### 'full ICU'

With the `--download=all`, this may download ICU if you don't have an
ICU in `deps/icu`.

Unix / Macintosh:

```text
$ ./configure --with-intl=full-icu --download=all
```

Windows:

```text
> vcbuild full-icu download-all
### no 'Intl'


The `Intl` object will not be available. This is the default at
present, so this option is not normally needed.

Unix / Macintosh:

```text
$ ./configure --with-intl=none
```

Windows:

```text
> vcbuild intl-none
```

### Existing 'ICU'

```text
$ pkg-config --modversion icu-i18n && ./configure --with-intl=system-icu
```

### Specific 'ICU'

You can find other ICU releases at
[the ICU homepage](http://icu-project.org/download).
Download the file named something like `icu4c-**##.#**-src.tgz` (or
`.zip`).

Unix / Macintosh

```text
# from an already-unpacked ICU:
$ ./configure --with-intl=[small-icu,full-icu] --with-icu-source=/path/to/icu

# from a local ICU tarball
$ ./configure --with-intl=[small-icu,full-icu] --with-icu-source=/path/to/icu.tgz

# from a tarball URL
$ ./configure --with-intl=full-icu --with-icu-source=http://url/to/icu.tgz
```

Windows

First unpack latest ICU to `deps/icu`
[icu4c-**##.#**-src.tgz](http://icu-project.org/download) (or `.zip`)
as `deps/icu` (You'll have: `deps/icu/source/...`)

```text
> vcbuild full-icu
```

### FIPS-compliant OpenSSL

NOTE: Windows is not yet supported

It is possible to build io.js with
[OpenSSL FIPS module](https://www.openssl.org/docs/fips/fipsnotes.html).

**Note** that building in this way does **not** allow you to
claim that the runtime is FIPS 140-2 validated.  Instead you
can indicate that the runtime uses a validated module.  See
the [security policy]
(http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140sp/140sp1747.pdf)
page 60 for more details.  In addition, the validation for
the underlying module is only valid if it is deployed in
accordance with its [security policy]
(http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140sp/140sp1747.pdf).
If you need FIPS validated cryptography it is recommended that you
read both the [security policy]
(http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140sp/140sp1747.pdf)
and [user guide] (https://openssl.org/docs/fips/UserGuide-2.0.pdf).

Instructions:

1. Obtain a copy of openssl-fips-x.x.x.tar.gz.
   To comply with the security policy you must ensure the path
   through which you get the file complies with the requirements
   for a "secure installation" as described in section 6.6 in
   the [user guide] (https://openssl.org/docs/fips/UserGuide-2.0.pdf).
   For evaluation/experimentation you can simply download and verify
   `openssl-fips-x.x.x.tar.gz` from https://www.openssl.org/source/
2. Extract source to `openssl-fips` folder and `cd openssl-fips`
3. `./config`
4. `make`
5. `make install`
   (NOTE: to comply with the security policy you must use the exact
   commands in steps 3-5 without any additional options as per
   Appendix A in the [security policy]
   (http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140sp/140sp1747.pdf).
   The only exception is that `./config no-asm` can be
   used in place of `./config` )
6. Get into io.js checkout folder
7. `./configure --openssl-fips=/path/to/openssl-fips/installdir`
   For example on ubuntu 12 the installation directory was
   /usr/local/ssl/fips-2.0
8. Build io.js with `make -j`
9. Verify with `node -p "process.versions.openssl"` (`1.0.2a-fips`)



## Security

All security bugs in io.js are taken seriously and should be reported by
emailing security@iojs.org. This will be delivered to a subset of the project
team who handle security issues. Please don't disclose security bugs
public until they have been handled by the security team.

Your email will be acknowledged within 24 hours, and you’ll receive a more
detailed response to your email within 48 hours indicating the next steps in
handling your report.

## Teams

The io.js project team comprises a group of core collaborators and a sub-group
that forms the _Technical Steering Committee_ (TSC) which governs the project. For more
information about the governance of the io.js project, see
[GOVERNANCE.md](./GOVERNANCE.md).

### Technical Steering Committee

* [bnoordhuis](https://github.com/bnoordhuis) - **Ben Noordhuis** &lt;info@bnoordhuis.nl&gt;
* [chrisdickinson](https://github.com/chrisdickinson) - **Chris Dickinson** &lt;christopher.s.dickinson@gmail.com&gt;
* [cjihrig](https://github.com/cjihrig) - **Colin Ihrig** &lt;cjihrig@gmail.com&gt;
* [fishrock123](https://github.com/fishrock123) - **Jeremiah Senkpiel** &lt;fishrock123@rocketmail.com&gt;
* [indutny](https://github.com/indutny) - **Fedor Indutny** &lt;fedor.indutny@gmail.com&gt;
* [jasnell](https://github.com/jasnell) - **James M Snell** &lt;jasnell@gmail.com&gt;
* [mhdawson](https://github.com/mhdawson) - **Michael Dawson** &lt;michael_dawson@ca.ibm.com&gt;
* [misterdjules](https://github.com/misterdjules) - **Julien Gilli** &lt;jgilli@nodejs.org&gt;
* [mscdex](https://github.com/mscdex) - **Brian White** &lt;mscdex@mscdex.net&gt;
* [orangemocha](https://github.com/orangemocha) - **Alexis Campailla** &lt;orangemocha@nodejs.org&gt;
* [piscisaureus](https://github.com/piscisaureus) - **Bert Belder** &lt;bertbelder@gmail.com&gt;
* [rvagg](https://github.com/rvagg) - **Rod Vagg** &lt;rod@vagg.org&gt;
* [shigeki](https://github.com/shigeki) - **Shigeki Ohtsu** &lt;ohtsu@iij.ad.jp&gt;
* [srl295](https://github.com/srl295) - **Steven R Loomis** &lt;srloomis@us.ibm.com&gt;
* [trevnorris](https://github.com/trevnorris) - **Trevor Norris** &lt;trev.norris@gmail.com&gt;

### Collaborators

* [brendanashworth](https://github.com/brendanashworth) - **Brendan Ashworth** &lt;brendan.ashworth@me.com&gt;
* [ChALkeR](https://github.com/ChALkeR) - **Сковорода Никита Андреевич** &lt;chalkerx@gmail.com&gt;
* [domenic](https://github.com/domenic) - **Domenic Denicola** &lt;d@domenic.me&gt;
* [evanlucas](https://github.com/evanlucas) - **Evan Lucas** &lt;evanlucas@me.com&gt;
* [geek](https://github.com/geek) - **Wyatt Preul** &lt;wpreul@gmail.com&gt;
* [isaacs](https://github.com/isaacs) - **Isaac Z. Schlueter** &lt;i@izs.me&gt;
* [jbergstroem](https://github.com/jbergstroem) - **Johan Bergström** &lt;bugs@bergstroem.nu&gt;
* [joaocgreis](https://github.com/joaocgreis) - **João Reis** &lt;reis@janeasystems.com&gt;
* [julianduque](https://github.com/julianduque) - **Julian Duque** &lt;julianduquej@gmail.com&gt;
* [lxe](https://github.com/lxe) - **Aleksey Smolenchuk** &lt;lxe@lxe.co&gt;
* [micnic](https://github.com/micnic) - **Nicu Micleușanu** &lt;micnic90@gmail.com&gt;
* [mikeal](https://github.com/mikeal) - **Mikeal Rogers** &lt;mikeal.rogers@gmail.com&gt;
* [monsanto](https://github.com/monsanto) - **Christopher Monsanto** &lt;chris@monsan.to&gt;
* [ofrobots](https://github.com/ofrobots) - **Ali Ijaz Sheikh** &lt;ofrobots@google.com&gt;
* [Olegas](https://github.com/Olegas) - **Oleg Elifantiev** &lt;oleg@elifantiev.ru&gt;
* [petkaantonov](https://github.com/petkaantonov) - **Petka Antonov** &lt;petka_antonov@hotmail.com&gt;
* [qard](https://github.com/qard) - **Stephen Belanger** &lt;admin@stephenbelanger.com&gt;
* [rlidwka](https://github.com/rlidwka) - **Alex Kocharin** &lt;alex@kocharin.ru&gt;
* [robertkowalski](https://github.com/robertkowalski) - **Robert Kowalski** &lt;rok@kowalski.gd&gt;
* [sam-github](https://github.com/sam-github) - **Sam Roberts** &lt;vieuxtech@gmail.com&gt;
* [seishun](https://github.com/seishun) - **Nikolai Vavilov** &lt;vvnicholas@gmail.com&gt;
* [silverwind](https://github.com/silverwind) - **Roman Reiss** &lt;me@silverwind.io&gt;
* [targos](https://github.com/targos) - **Michaël Zasso** &lt;mic.besace@gmail.com&gt;
* [tellnes](https://github.com/tellnes) - **Christian Tellnes** &lt;christian@tellnes.no&gt;
* [thefourtheye](https://github.com/thefourtheye) - **Sakthipriyan Vairamani** &lt;thechargingvolcano@gmail.com&gt;
* [thlorenz](https://github.com/thlorenz) - **Thorsten Lorenz** &lt;thlorenz@gmx.de&gt;
* [Trott](https://github.com/Trott) - **Rich Trott** &lt;rtrott@gmail.com&gt;
* [tunniclm](https://github.com/tunniclm) - **Mike Tunnicliffe** &lt;m.j.tunnicliffe@gmail.com&gt;
* [vkurchatkin](https://github.com/vkurchatkin) - **Vladimir Kurchatkin** &lt;vladimir.kurchatkin@gmail.com&gt;
* [yosuke-furukawa](https://github.com/yosuke-furukawa) - **Yosuke Furukawa** &lt;yosuke.furukawa@gmail.com&gt;

Collaborators & TSC members follow the [COLLABORATOR_GUIDE.md](./COLLABORATOR_GUIDE.md) in
maintaining the io.js project.

### Release

Releases of Node.js and io.js will be signed with one of the following GPG keys:

* **Chris Dickinson** &lt;christopher.s.dickinson@gmail.com&gt;: `9554F04D7259F04124DE6B476D5A82AC7E37093B`
* **Colin Ihrig** &lt;cjihrig@gmail.com&gt; `94AE36675C464D64BAFA68DD7434390BDBE9B9C5`
* **Sam Roberts** &lt;octetcloud@keybase.io&gt; `0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93`
* **Jeremiah Senkpiel** &lt;fishrock@keybase.io&gt; `FD3A5288F042B6850C66B31F09FE44734EB7990E`
* **James M Snell** &lt;jasnell@keybase.io&gt; `71DCFD284A79C3B38668286BC97EC7A07EDE3FC1`
* **Rod Vagg** &lt;rod@vagg.org&gt; `DD8F2338BAE7501E3DD5AC78C273792F7D83545D`

The full set of trusted release keys can be imported by running:

```
gpg --keyserver pool.sks-keyservers.net --recv-keys 9554F04D7259F04124DE6B476D5A82AC7E37093B
gpg --keyserver pool.sks-keyservers.net --recv-keys 94AE36675C464D64BAFA68DD7434390BDBE9B9C5
gpg --keyserver pool.sks-keyservers.net --recv-keys 0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93
gpg --keyserver pool.sks-keyservers.net --recv-keys FD3A5288F042B6850C66B31F09FE44734EB7990E
gpg --keyserver pool.sks-keyservers.net --recv-keys 71DCFD284A79C3B38668286BC97EC7A07EDE3FC1
gpg --keyserver pool.sks-keyservers.net --recv-keys DD8F2338BAE7501E3DD5AC78C273792F7D83545D
```

See the section above on [Download Verification](#verifying-binaries) for
details on what to do with these keys to verify that a downloaded file is official.

Previous releases of Node.js have been signed with one of the following GPG
keys:

* Julien Gilli &lt;jgilli@fastmail.fm&gt; `114F43EE0176B71C7BC219DD50A3051F888C628D`
* Timothy J Fontaine &lt;tjfontaine@gmail.com&gt; `7937DFD2AB06298B2293C3187D33FF9D0246406D`
* Isaac Z. Schlueter &lt;i@izs.me&gt; `93C7E9E91B49E432C2F75674B0A78B0A6C481CF6
