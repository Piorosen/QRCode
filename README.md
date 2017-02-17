# QRCode - A QR Code Generator in Swift

[![Build Status](https://travis-ci.org/dmrschmidt/QRCode.svg?branch=master)](https://travis-ci.org/dmrschmidt/QRCode/)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

A simple QR code image generator to use in your apps, written in Swift 3.

# Installation

## Carthage

Simply add the following to your Cartfile and run `carthage update`:

```
github "dmrschmidt/QRCode", ~> 0.5.0
```

# Usage

You can create a `QRCode` from either `(NS)Data`, `(NS)String` or `(NS)URL`:

```swift
// create a QRCode with all the default values
let qrCodeA = QRCode(data: myData)
let qrCodeB = QRCode(string: "my awesome QR code")
let qrCodeC = QRCode(url: URL(string: "https://example.com"))
```

To get the `UIImage` representation of your created qrCode, simply call it's
`image` method:

```swift
let myImage: UIImage? = try? qrCode.image()
```

If you provide a desired `size` for the output image (see Customization below),
this method can throw, in case the desired image size is too small for the data
being provided, i.e. some pixels would need to be omitted during scaling.

There is an alternative attribute `unsafeImage` accessible, which will simply
return `nil` in these cases. If you never specify any custom size however, you
could use `unsafeImage` instead, since the image will automatically pick the
ideal size.

To show the `QRCode` in a `UIImageView`, and if you're a fan of extensions,
you may want to consider creating an extension in your app like so:

```swift
extension UIImageView {
    convenience init(qrCode: QRCode) {
        self.init(image: qrCode.unsafeImage)
    }    
}
```

## Customization

A `QRCode` image can be customized in the following ways:

```swift
// As an immutable let, by setting all up in the respective constructors.
// This is the recommended approach.
let qrCode = QRCode(string: "my customized QR code",
                    color: UIColor.red,
                    backgroundColor: UIColor.green,
                    imageSize: CGSize(width: 100, height: 100),
                    scale: 1.0,
                    inputCorrection: .medium)

// As a mutable var, by setting the individual parameters.
var qrCode = QRCode(string: "my customizable QR code")
qrCode.color = UIColor.red // image foreground (or actual code) color
qrCode.backgroundColor = UIColor.blue // image background color
qrCode.size = CGSize(width: 300, height: 300) // final scaled image size
qrCode.scale = 1.0 // image scaling factor
qrCode.inputCorrection = .quartile // amount of error correction information added
```

![Screenshot](https://github.com/dmrschmidt/QRCode/blob/master/screenshot.png)
