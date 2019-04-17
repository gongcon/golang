# Learning golang
## IDE

### Install GoEclipse
1. Eclipse 2018-09
1. Have Marketplace Client (mpc) installed
1. Open mpc via **Help -> Eclipse Marketplace...**
1. Drag & Drop the Install button in https://marketplace.eclipse.org/content/goclipse to the mpc window
1. Setup GOPATH
  * `go env | grep GOPATH` to view the GOPATH
  * Open **Preferences -> Go**, setup the Go installtion directory and Cclipse GOPATH
1. Configuration
  * Run in command line: `go get -u github.com/mdempsky/gocode`
  * Open **Preferences -> Go -> Tools**
  * Paste the executable path to gocode
  * Click **Download** button in guru
  * Click **Download** button in godef
 
### Debugging
Here are the steps to installing and setting up GDB on Mac OS Sierra/High Sierra.
Run `brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/c3128a5c335bd2fa75ffba9d721e9910134e4644/Formula/gdb.rb` to install a version (8.01) of gdb with the patch for MacOS. If the latest version of gdb (so far it's 8.2) was installed, unstall it by using `brew install gdb`.
On starting gdb, you will get the following error:
```bash
Unable to find Mach task port for process-id 2133: (os/kern) failure (0x5).
 (please check gdb is codesigned - see taskgated(8))
```

To fix this error, follow the following steps:

1. Open Keychain Access
1. In menu, open **Keychain Access > Certificate Assistant > Create a Certificate**
1. Give it a name (e.g. `gdb-cert`)
  + Identity type: Self Signed Root
  + Certificate type: Code Signing
  + Check: Let Me Override Defaults
1. Continue until "Specify a Location For"
1. Set Keychain location to System. If this yields the following error:
`Certificate Error: Unknown Error =-2,147,414,007`
Set Location to Login, Unlock System by click on the lock at the top left corner and drag and drop the certificate gdbcert to the System Keychain.
1. Create certificate and close Certificate Assistant.
1. Find the certificate in System keychain.
1. Double click certificate.
1. Expand **Trust**, set **Code signing** to `Always Trust`
1. Restart taskgated in terminal: `killall taskgated`
1. Codesign gdb using your certificate: `codesign -fs gdb-cert /usr/local/bin/gdb`
1. Add "set startup-with-shell off" to `~/.gdbinit` file to avoid a message when running: "During startup program terminated with signal ?, unkown signal."

## OOP
Golang does not have `class` like Java. It uses `struct` to define the variables of a `class` and associates `func` to it, for example,

```
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

In this example, the function `Scale` changes the values of `X` and `Y` of the `Vertex`. It must be defined on `*Vertex` rather than `Vertex`. Otherwise, it will be passed by value (a copy of the original `Vertext` value) and won't get the changes.
