# Postman Dynamic Variables Reference

Postman resolves these variables at request send time. Use them inside double curly braces directly in curl values (e.g., `{{$guid}}`).

## Identity & Unique IDs
| Variable | Description | Example output |
|---|---|---|
| `{{$guid}}` | UUID v4 | `110e8400-e29b-41d4-a716-446655440000` |
| `{{$randomUUID}}` | UUID v4 (alias) | same as above |

## Time
| Variable | Description | Example output |
|---|---|---|
| `{{$timestamp}}` | Unix timestamp (seconds) | `1718200000` |
| `{{$isoTimestamp}}` | ISO 8601 UTC | `2024-06-12T14:00:00.000Z` |
| `{{$randomDateFuture}}` | Random future date | ISO string |
| `{{$randomDatePast}}` | Random past date | ISO string |
| `{{$randomDateRecent}}` | Random recent date | ISO string |

## Numbers
| Variable | Description | Example output |
|---|---|---|
| `{{$randomInt}}` | Integer 0–1000 | `42` |
| `{{$randomFloat}}` | Float 0–1000 | `123.45` |
| `{{$randomPrice}}` | Price with decimals | `456.99` |
| `{{$randomPhoneNumber}}` | Phone number string | `700-555-0158` |
| `{{$randomPhoneNumberExt}}` | Phone number with extension | `700-555-0158 x5589` |

## Boolean
| Variable | Description | Example output |
|---|---|---|
| `{{$randomBoolean}}` | true or false | `true` |

## Strings / Text
| Variable | Description | Example output |
|---|---|---|
| `{{$randomAlphaNumeric}}` | Single alphanumeric char | `a` |
| `{{$randomWord}}` | Single word | `Coordinator` |
| `{{$randomWords}}` | 3–5 random words | `Synchronised next-generation collaboration` |
| `{{$randomLoremWord}}` | Lorem ipsum word | `lorem` |
| `{{$randomLoremSentence}}` | Lorem ipsum sentence | `Lorem ipsum...` |
| `{{$randomLoremParagraph}}` | Lorem ipsum paragraph | — |

## Person
| Variable | Description | Example output |
|---|---|---|
| `{{$randomFirstName}}` | First name | `John` |
| `{{$randomLastName}}` | Last name | `Doe` |
| `{{$randomFullName}}` | Full name | `John Doe` |
| `{{$randomNamePrefix}}` | Name prefix | `Mr.` |
| `{{$randomNameSuffix}}` | Name suffix | `Jr.` |
| `{{$randomJobTitle}}` | Job title | `Principal Directives Engineer` |
| `{{$randomJobDescriptor}}` | Job descriptor | `Legacy` |
| `{{$randomJobArea}}` | Job area | `Accountability` |
| `{{$randomJobType}}` | Job type | `Supervisor` |

## Internet / Contact
| Variable | Description | Example output |
|---|---|---|
| `{{$randomEmail}}` | Email address | `user@example.com` |
| `{{$randomUserName}}` | Username | `john_doe` |
| `{{$randomUrl}}` | URL | `https://example.com` |
| `{{$randomDomainName}}` | Domain name | `example.com` |
| `{{$randomDomainSuffix}}` | Domain suffix | `.com` |
| `{{$randomDomainWord}}` | Domain word | `example` |
| `{{$randomIp}}` | IPv4 address | `192.168.0.1` |
| `{{$randomIPV6}}` | IPv6 address | `2001:db8::1` |
| `{{$randomMACAddress}}` | MAC address | `01:23:45:67:89:ab` |
| `{{$randomUserAgent}}` | User agent string | `Mozilla/5.0 ...` |

## Location
| Variable | Description | Example output |
|---|---|---|
| `{{$randomCity}}` | City name | `London` |
| `{{$randomCountry}}` | Country name | `United Kingdom` |
| `{{$randomCountryCode}}` | ISO country code | `GB` |
| `{{$randomLocale}}` | Locale string | `en-GB` |
| `{{$randomZipCode}}` | Zip/postal code | `SW1A 1AA` |
| `{{$randomStreetName}}` | Street name | `Main Street` |
| `{{$randomStreetAddress}}` | Full street address | `123 Main Street` |
| `{{$randomLatitude}}` | Latitude | `51.5074` |
| `{{$randomLongitude}}` | Longitude | `-0.1278` |

## Finance
| Variable | Description | Example output |
|---|---|---|
| `{{$randomBankAccount}}` | Bank account number | `12345678` |
| `{{$randomBankAccountName}}` | Account name | `Checking Account` |
| `{{$randomCreditCardMask}}` | Masked credit card | `xxxx-xxxx-xxxx-1234` |
| `{{$randomBankAccountIban}}` | IBAN | `GB29NWBK60161331926819` |
| `{{$randomBankAccountBic}}` | BIC/SWIFT | `MIDLGB22` |
| `{{$randomCurrencyCode}}` | ISO currency code | `GBP` |
| `{{$randomCurrencyName}}` | Currency name | `British Pound` |
| `{{$randomCurrencySymbol}}` | Currency symbol | `£` |

## Colour / Image
| Variable | Description | Example output |
|---|---|---|
| `{{$randomHexColor}}` | Hex colour | `#a3c4bc` |
| `{{$randomRGBColor}}` | RGB colour | `rgb(163, 196, 188)` |
| `{{$randomAbbreviation}}` | Random abbreviation | `SMTP` |

## Environment-scoped variables (user-defined)
Use `{{variable_name}}` (without `$`) to reference variables defined in a Postman environment or collection.

Common conventions used in this codebase:
| Variable | Typical use |
|---|---|
| `{{baseUrl}}` | Service base URL |
| `{{authToken}}` | Bearer token |
| `{{correlationId}}` | Correlation / trace ID header |
| `{{msisdn}}` | Subscriber MSISDN |
| `{{accountId}}` | Account identifier |
