---
title: "Currency FormatStyle"
date: 2022-03-15T22:17:39-06:00
draft: false
tags: [ios15, formatstyle, deepdive, development, swift, swiftui]
---

[This is part of the FormatStyle Deep Dive series](/posts/formatstyle-deep-dive)

[See the other numerical formatters](/posts/formatstyle/numerical/)

The intricacies of displaying money are fully supported by this `FormatStyle`.

The examples below show the individual options available to format your final string, the real power available is that you chain these options together to allow for a truly staggering amount of customization.

<hr>

[Download the Xcode Playground with all examples](https://github.com/brettohland/FormatStylesDeepDive/)

[See the examples as a gist](https://gist.github.com/brettohland/ac2fbd1446bc7bb64da491587b010e3c)

<hr>

```Swift
// MARK: - Grouping

Int(3_000).formatted(.currency(code: "GBP").grouping(.never)) // "£3000.00"
Int(3_000).formatted(.currency(code: "GBP").grouping(.automatic)) // "£3,000.00"

// MARK: - Precision

// Please don't use Floating point numbers to store currency. Please.
Float(3_000.003).formatted(.currency(code: "GBP").precision(.fractionLength(4))) // "£3,000.0029" <- This is why
Float(3_000.003).formatted(.currency(code: "GBP").precision(.fractionLength(1 ... 4))) // "£3,000.0029"

Decimal(3_000.003).formatted(.currency(code: "GBP").precision(.fractionLength(4)))       // "£3,000.0029"
Decimal(3_000.003).formatted(.currency(code: "GBP").precision(.fractionLength(1 ... 4))) // "£3,000.0029"

Decimal(3).formatted(.currency(code: "GBP").precision(.integerAndFractionLength(integer: 4, fraction: 4))) // "£0,003.0000"
Decimal(3).formatted(
    .currency(code: "GBP")
    .precision(.integerAndFractionLength(integerLimits: 1 ... 5, fractionLimits: 1 ... 5))
) // "£3.0"
Decimal(3.00004).formatted(
    .currency(code: "GBP")
    .precision(.integerAndFractionLength(integerLimits: 1 ... 5, fractionLimits: 1 ... 5))
) // "£3.00004"
Decimal(3.000000004).formatted(
    .currency(code: "GBP")
    .precision(.integerAndFractionLength(integerLimits: 1 ... 5, fractionLimits: 1 ... 5))
)
Decimal(30000.01).formatted(
    .currency(code: "GBP")
    .precision(.integerAndFractionLength(integerLimits: 1 ... 5, fractionLimits: 1 ... 5))
) // "£30,000.01"
Decimal(3000000.000001).formatted(
    .currency(code: "GBP")
    .precision(.integerAndFractionLength(integerLimits: 1 ... 5, fractionLimits: 1 ... 5))
) // "£0.0"

Decimal(10.1).formatted(.currency(code: "GBP").precision(.significantDigits(1))) // "£10"
Decimal(10.1).formatted(.currency(code: "GBP").precision(.significantDigits(2))) // "£10"
Decimal(10.1).formatted(.currency(code: "GBP").precision(.significantDigits(3))) // "£10.1"
Decimal(10.1).formatted(.currency(code: "GBP").precision(.significantDigits(4))) // "£10.10"
Decimal(10.1).formatted(.currency(code: "GBP").precision(.significantDigits(5))) // "£10.100"

Decimal(1).formatted(.currency(code: "GBP").precision(.significantDigits(1 ... 3)))     // "£1"
Decimal(10).formatted(.currency(code: "GBP").precision(.significantDigits(1 ... 3)))    // "£10"
Decimal(10.1).formatted(.currency(code: "GBP").precision(.significantDigits(1 ... 3)))  // "£10.1"
Decimal(10.01).formatted(.currency(code: "GBP").precision(.significantDigits(1 ... 3))) // "£10"

// MARK: - Decimal Separator

Decimal(3000).formatted(.currency(code: "GBP").decimalSeparator(strategy: .automatic)) // "£3,000.00"
Decimal(3000).formatted(.currency(code: "GBP").decimalSeparator(strategy: .always))    // "£3,000.00"

// MARK: - Presentation

Decimal(10).formatted(.currency(code: "GBP").presentation(.fullName)) // "10.00 British pounds"
Decimal(10).formatted(.currency(code: "GBP").presentation(.isoCode))  // "GBP 10.00"
Decimal(10).formatted(.currency(code: "GBP").presentation(.narrow))   // "£10.00"
Decimal(10).formatted(.currency(code: "GBP").presentation(.standard)) // "£10.00"

// MARK: - Presentation with Locale

Decimal(10).formatted(.currency(code: "GBP").presentation(.fullName).locale(Locale(identifier: "fr_FR"))) // "10,00 livres sterling"

// MARK: - Rounded

Decimal(0.59).formatted(.currency(code: "GBP").rounded())   // "£0.59"
Decimal(0.599).formatted(.currency(code: "GBP").rounded())  // "£0.60"
Decimal(0.5999).formatted(.currency(code: "GBP").rounded()) // "£0.60"

Decimal(5.001).formatted(.currency(code: "GBP").rounded(rule: .awayFromZero)) // "£5.01"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .awayFromZero))  // "£5.01"

Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .awayFromZero, increment: 1))  // "£6"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .awayFromZero, increment: 10)) // "£10"

Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .down))                    // "£5.00"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .toNearestOrAwayFromZero)) // "£5.01"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .towardZero))              // "£5.00"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .up))                      // "£5.01"

Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .down, increment: 1)) // "£5"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .toNearestOrAwayFromZero, increment: 1)) // "£5"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .towardZero, increment: 1)) // "£5"
Decimal(5.01).formatted(.currency(code: "GBP").rounded(rule: .up, increment: 1)) // "£5"

// MARK: - Sign

Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .automatic))                         // "£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .never))                             // "£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .accounting))                        // "£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .accountingAlways()))                // "+£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .accountingAlways(showZero: true)))  // "+£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .accountingAlways(showZero: false))) // "+£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .always()))                          // "+£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .always(showZero: true)))            // "+£7.00"
Decimal(7).formatted(.currency(code: "GBP").sign(strategy: .always(showZero: false)))           // "+£7.00"

Decimal(10).formatted(
    .currency(code: "GBP")
    .scale(200.0)
    .sign(strategy: .always())
    .presentation(.fullName)
) // "+2,000.00 British pounds"
```

<hr>

## Attributed String Output

`AttributedString` output can be had by this formatter by adding the `.attributed` parameter to the `FormatStyle` during use.

You can read more details in the [AttributedString Output deep-dive](/posts/formatstyle/style-deep-dives/attributed-strings/)

<hr>

[Download the Xcode Playground with all examples](https://github.com/brettohland/FormatStylesDeepDive/)

[See the examples as a gist](https://gist.github.com/brettohland/ac2fbd1446bc7bb64da491587b010e3c)

<hr>