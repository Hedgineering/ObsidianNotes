# Security Identification Numbers

> [!abstract] Why this matters Every security master needs a spine of unique identifiers to avoid trading the wrong instrument, mismatching corporate actions, or breaking settlement. This note covers the identifiers you'll meet most often — [[#ISIN|ISIN]], [[#CUSIP|CUSIP]], [[#SEDOL|SEDOL]], [[#LEI|LEI]], and [[#FIGI|FIGI]] — plus a few adjacent codes worth knowing ([[#Other identifiers worth knowing|CIK, tickers, WKN]]). Each section decomposes the code, explains the check-digit math, and includes tested Python.

## Map of the territory

|Identifier|Scope|Identifies|Length|Check digit?|Issuing body|
|---|---|---|---|---|---|
|[[#ISIN]]|Global|Any security (equity, debt, fund, etc.)|12 chars|1 (Luhn/mod-10)|National Numbering Agencies (NNAs), coordinated via ANNA|
|[[#CUSIP]]|US & Canada|Securities and issuers|9 chars|1 (mod-10 double-add-double)|CUSIP Global Services (CGS), operated by FactSet for the ABA|
|[[#SEDOL]]|UK & Ireland (embedded in ISINs globally)|Securities, per-exchange listing|7 chars|1 (weighted mod-10)|London Stock Exchange|
|[[#LEI]]|Global|**Legal entities**, not securities|20 chars|2 (ISO 7064 mod-97-10)|Local Operating Units (LOUs) under GLEIF|
|[[#FIGI]]|Global|Any instrument, per venue + composite|12 chars|1 (mod-10 double-add-double variant)|Bloomberg, as OMG-designated Registration Authority (open standard)|

> [!tip] The single most important distinction An **LEI identifies who** (the issuing or counterparty legal entity). Every other code on this page **identifies what** (the security/instrument itself). A company can have one LEI but dozens of CUSIPs/ISINs (one per share class, bond, or listing).

---

## ISIN

**International Securities Identification Number** — ISO 6166. Think of it as a security's international passport: a wrapper that takes a country's local identifier (CUSIP, SEDOL, WKN, etc.) and makes it globally addressable.

### Structure

|Position|Length|Field|Description|
|---|---|---|---|
|1–2|2|Country code|ISO 3166-1 alpha-2 code of the country where the issuer is legally registered (for equities) — **not necessarily where the company is headquartered or listed**|
|3–11|9|NSIN (National Securities Identifying Number)|The local identifier. For US/Canada this **is** the CUSIP; for the UK, a zero-padded SEDOL; other countries use their own NNA-assigned scheme|
|12|1|Check digit|Luhn (mod-10) checksum over the whole 11-character body, after converting letters to numbers|

For any US or Canadian security, the ISIN is just the CUSIP with a 2-letter country-code prefix and a Luhn check digit added, so US0378331005 is Apple Inc., where 037833100 is the CUSIP, US the issuing country, and 5 the computed check digit.

> [!note] Country code ≠ listing venue A depositary receipt (e.g. an ADR) is coded with the country of the entity that _issued the receipt_, not the underlying company's home country. Always check this before assuming ISIN prefix = "where the stock trades."

### Real examples

|ISIN|Company|Country code|NSIN (body)|
|---|---|---|---|
|`US0378331005`|Apple Inc. (common stock)|US|037833100 (its CUSIP)|
|`US5949181045`|Microsoft Corp.|US|594918104|
|`GB0002634946`|BAE Systems plc|GB|0002634 9 → padded SEDOL `0263494`|
|`DE0007164600`|SAP SE|DE|0007164 6 (German WKN-derived)|
|`JP3633400001`|Toyota Motor Corp.|JP|local Japanese code|

### Check-digit algorithm (Luhn / modulus 10)

[Useful Website with info and algo checker for CUSIP to ISIN conversions](https://alltools.dev/tools/finance/cusip-to-isin-converter/)

The standard description of the steps: starting from the rightmost digit, double the value of every second digit, and if the result is greater than 9, subtract 9 from it, then sum everything and take the complement to the next multiple of 10.

```python
def isin_check_digit(body11: str) -> int:
    """Compute the Luhn (mod-10) check digit for an 11-character
    ISIN body (2-char country code + 9-char NSIN)."""
    # Step 1: letters -> numbers (A=10 ... Z=35), digits stay as-is
    digits = "".join(
        c if c.isdigit() else str(ord(c.upper()) - ord("A") + 10)
        for c in body11
    )
    # Step 2: Luhn, working from the rightmost digit
    total = 0
    for i, d in enumerate(reversed(digits)):
        n = int(d)
        if i % 2 == 0:          # 0-indexed from the right: 1st, 3rd, 5th... digit doubled
            n *= 2
            if n > 9:
                n -= 9
        total += n
    return (10 - total % 10) % 10


def isin_validate(isin: str) -> bool:
    isin = isin.strip().upper()
    if len(isin) != 12:
        return False
    return isin_check_digit(isin[:11]) == int(isin[11])


# Apple Inc.
print(isin_check_digit("US037833100"))   # -> 5
print(isin_validate("US0378331005"))     # -> True
print(isin_validate("US0378331009"))     # -> False (wrong check digit)
```

**Sources:** [ISIN — isin.com](https://www.isin.com/isin/) · [Luhn algorithm — Wikipedia](https://en.wikipedia.org/wiki/Luhn_algorithm) · [ISIN checksum walkthrough — FasterCapital](https://fastercapital.com/content/ISIN-Checksum--Verifying-the-Validity-of-an-ISIN-Code.html) · [CUSIP→ISIN converter, worked example](https://alltools.dev/tools/finance/cusip-to-isin-converter/)

---

## CUSIP

**Committee on Uniform Securities Identification Procedures** number. CUSIP stands for Committee on Uniform Securities Identification Procedures, and a CUSIP number identifies most financial instruments, including stocks of all registered U.S. and Canadian companies, commercial paper, and U.S. government and municipal bonds. CUSIP Global Services is owned by the American Bankers Association and managed by Standard & Poor's Global Market Intelligence, and it helps facilitate the clearance and settlement of securities.

### Structure

|Position|Length|Field|Example (Apple common stock: `037833100`)|
|---|---|---|---|
|1–6|6|Issuer number|`037833` = Apple Inc.|
|7–8|2|Issue number|`10` = common stock|
|9|1|Check digit|`0`|

The first 6 digits of a CUSIP identify the company, digits 7-8 describe the security, and the 9th is a check digit. The first equity security a company issues receives the digits 10, and additional issues increment by 10 — so adding 10 to the 6-digit company identifier will usually return the common stock, and if that fails, try adding 20 instead. The issue identifier in positions 7 and 8 is what distinguishes share classes — for example, positions 7-8 of `30` versus `10` distinguish voting from non-voting share classes issued by the same company, since the first six characters stay identical when the issuer is the same.

### Issue-number ranges (the "what kind of security is this" code)

> [!warning] These ranges are conventions, not a strict published law CUSIP issue numbers are assigned by CGS without a single universal public rulebook, but the following patterns are well documented and widely relied upon in practice.

|Issue number range|Typical security type|Notes|
|---|---|---|
|`10`–`19`|Common stock|Issue numbers 10-19 typically indicate common stock. First class of common issued gets `10`; a second share class often gets `30` or similar|
|`20`–`29`|Preferred stock|Issue numbers 20-29 indicate preferred stock.|
|`30`–`39`|Additional equity classes / warrants (varies by issuer)|Often used for a second common share class, e.g. Alphabet Class C|
|`40`–`49`|Corporate bonds / debt|Issue numbers 40-49 indicate corporate bonds. Debt issues more commonly use **letters** rather than pure numbers|
|Letters (e.g. `AA`, `AB`, `AK`)|Bonds / debentures|Debt securities like bonds typically use letters (e.g. AA for a double-A rated bond) whereas equity securities use numbers (e.g. 10 for common stock).|
|`#`, `@`, `*` in position 6, 7, or 8|Private Placement Number (PPN)|PPNs are recognized by a #, @, or * in the 6th, 7th, or 8th position — a # in the 6th position of a privately traded issuer, or in the 7th/8th position for privately traded issues of otherwise public entities.|

### Real examples

|CUSIP|Company / instrument|Issuer code|Issue code|
|---|---|---|---|
|`037833100`|Apple Inc. — common stock|`037833`|`10`|
|`594918104`|Microsoft Corp. — common stock|`594918`|`10`|
|`02079K107`|Alphabet Inc. — Class A (voting)|`02079K`|`10`|
|`02079K305`|Alphabet Inc. — Class C (non-voting)|`02079K`|`30`|
|`931142103`|Walmart Inc. — common stock|`931142`|`10`|
|`037833AK` + check|Apple Inc. — a specific bond issue|`037833`|`AK` (letters = debt)|

Every US Treasury issuance gets its own CUSIP — a 10-year note auctioned on one date has a different CUSIP than a 10-year note auctioned on another date, because the coupon and maturity differ.

### CINS — the international sibling

The CUSIP International Numbering System (CINS) is a 9-character alphanumeric identifier that uses the same 9 characters as CUSIP but also contains a letter in the first position signifying the issuer's country or geographic region, developed in 1989 in response to U.S. demand for global coverage.

### Check-digit algorithm (Modulus 10, Double-Add-Double)

The technique used is documented as: Modulus 10 Double Add Double, weighting every second digit by 2, with alphabetic characters converted to numbers by their position in the alphabet starting with A=10, and any weighted value greater than nine treated as two separate digits.

```python
def cusip_char_value(c: str) -> int:
    """Map a CUSIP character to its numeric value. A=10..Z=35;
    the rarely-used private-placement symbols * @ # map to 36/37/38."""
    if c.isdigit():
        return int(c)
    if c.isalpha():
        return ord(c.upper()) - ord("A") + 10
    return {"*": 36, "@": 37, "#": 38}[c]


def cusip_check_digit(body8: str) -> int:
    """Compute the check digit for the first 8 characters of a CUSIP."""
    total = 0
    for i, c in enumerate(body8):
        v = cusip_char_value(c)
        if (i + 1) % 2 == 0:      # double every 2nd, 4th, 6th, 8th character
            v *= 2
        total += v // 10 + v % 10  # "double-add-double": split 2-digit results and add
    return (10 - total % 10) % 10


def cusip_validate(cusip: str) -> bool:
    cusip = cusip.strip().upper()
    if len(cusip) != 9:
        return False
    return cusip_check_digit(cusip[:8]) == int(cusip[8])


for c in ["037833100", "17275R102", "594918104", "02079K107", "02079K305"]:
    print(c, cusip_validate(c))   # all -> True
```

**Sources:** [CUSIP — isin.com](https://www.isin.com/cusip/) · [Apache Commons Validator — CUSIPCheckDigit](https://commons.apache.org/validator/apidocs/org/apache/commons/validator/routines/checkdigit/CUSIPCheckDigit.html) · [python-stdnum cusip.py](https://github.com/arthurdejong/python-stdnum/blob/master/stdnum/cusip.py) · [SEC.gov — What is a CUSIP](https://www.sec.gov/answers/cusip) · [CUSIP Global Services — About CGS identifiers](https://www.cusip.com/identifiers.html)

---

## SEDOL

**Stock Exchange Daily Official List** number — the UK/Ireland national identifier, issued by the London Stock Exchange. It also shows up embedded inside GB-prefixed ISINs.

### Structure

|Position|Length|Field|
|---|---|---|
|1–6|6|Alphanumeric issue code (digits and consonant letters only — **no vowels**, to avoid accidentally spelling words)|
|7|1|Check digit|

SEDOLs issued before January 26, 2004 were purely numeric — those from Asia and Africa typically began with 6, UK and Ireland ones with 0 or 3, the rest of Europe with 4, 5, or 7, and the Americas with 2. After that date SEDOLs became alphanumeric and are issued sequentially starting from B000009, with numbers preceding letters at each position and vowels never used, so all new SEDOLs begin with a letter; ranges beginning with 9 are reserved for end-user allocation.

> [!example] Old-style codes still work BP's SEDOL 0798059, issued before the 2004 format change, still identifies its London-listed ordinary shares today, alongside newer alphanumeric codes in the same system.

### Real examples

|SEDOL|Company|
|---|---|
|`0798059`|BP plc (pre-2004 numeric style)|
|`0263494`|BAE Systems plc|
|`B0YBKJ7` (`B0YBKJ` + check digit `7`)|Royal Dutch Shell (example alphanumeric code)|
|`710889` + check digit `9` → `7108899`|Example from the classic Rosetta Code reference set|

### Check-digit algorithm (weighted modulus 10)

The check digit is chosen so the total weighted sum of all seven characters is a multiple of 10, computed from a weighted sum of the first six characters where letters have the value of 9 plus their alphabet position, so B=11 and Z=35 — and vowels, while never used, are not skipped when assigning these values (e.g. H=17, J=19), which simplifies the calculation. The weights, applied left to right: 1, 3, 1, 7, 3, 9, 1 (the 7th weight applies to the check digit itself and isn't needed when _computing_ it).

```python
# Vowels are skipped when listing valid characters, but NOT when
# computing their numeric value (H=17, J=19 even though I is unused).
_SEDOL_ALPHABET = "0123456789 BCD FGH JKLMN PQRST VWXYZ"  # spaces mark unused vowel slots

def sedol_char_value(c: str) -> int:
    return _SEDOL_ALPHABET.index(c.upper())


def sedol_check_digit(body6: str) -> int:
    weights = (1, 3, 1, 7, 3, 9)
    total = sum(w * sedol_char_value(c) for w, c in zip(weights, body6))
    return (10 - total % 10) % 10


def sedol_validate(sedol: str) -> bool:
    sedol = sedol.strip().upper()
    if len(sedol) != 7:
        return False
    return sedol_check_digit(sedol[:6]) == int(sedol[6])


print(sedol_check_digit("710889"))   # -> 9  (BP-style historical example)
print(sedol_check_digit("026349"))   # -> 4  (BAE Systems: 0263494)
print(sedol_check_digit("B0YBKJ"))   # -> 7
```

**Sources:** [SEDOL — Wikipedia](https://en.wikipedia.org/wiki/SEDOL) · [python-stdnum gb/sedol.py](https://github.com/arthurdejong/python-stdnum/blob/master/stdnum/gb/sedol.py) · [Apache Commons Validator — SedolCheckDigit](https://commons.apache.org/proper/commons-validator/jacoco/org.apache.commons.validator.routines.checkdigit/SedolCheckDigit.java.html) · [SEDOL explainer — marketgenius.app](https://marketgenius.app/articles/explainers/sedol-code-uk-stock-identifier-behind-every-trade)

---

## LEI

**Legal Entity Identifier** — ISO 17442. This is the odd one out on this page: it identifies the _entity_, not a specific security. Born from a G20/Financial Stability Board push after the 2008 crisis, when regulators struggled to map Lehman Brothers' exposures across hundreds of subsidiaries with no shared identifier.

### Structure

|Position|Length|Field|
|---|---|---|
|1–4|4|LOU (Local Operating Unit) prefix — identifies the accredited issuer of the code|
|5–6|2|Reserved, always `00`|
|7–18|12|Entity-specific, semantically meaningless string|
|19–20|2|Check digits|

The LEI is formatted as a 20-character alphanumeric code based on the ISO 17442 standard, where the first four characters identify the Local Operating Unit (LOU) that issued it, characters 5 to 18 are the unique alphanumeric string assigned by the LOU, and the final two characters are checksum digits calculated using MOD-97-10 per ISO/IEC 7064.

> [!note] Opaque by design Unlike a CUSIP, the entity block carries no embedded meaning about the company's name or country — it's opaque by design, so the code never changes when a company renames or relocates.

### Real example

|LEI|Entity|
|---|---|
|`HWUPKR0MPOU8FGXBT394`|Apple Inc.|

### Check-digit algorithm (ISO 7064, MOD 97-10)

Step 1: any letters in the 18 alphanumeric characters are converted to digit pairs; Step 2: two zeros are appended at the rightmost positions; Step 3: a Euclidean division of the resulting number by 97 is performed; Step 4: the remainder is subtracted from 98 to determine the check digit pair. A fully valid 20-character LEI, run through the same conversion and divided by 97, leaves a remainder of "1".

```python
def _lei_letters_to_digits(s: str) -> str:
    """A=10, B=11, ... Z=35; digits pass through unchanged."""
    return "".join(
        c if c.isdigit() else str(ord(c.upper()) - ord("A") + 10)
        for c in s
    )


def lei_check_digits(body18: str) -> int:
    numeric = _lei_letters_to_digits(body18) + "00"
    remainder = int(numeric) % 97
    return 98 - remainder


def lei_validate(lei: str) -> bool:
    lei = lei.strip().upper()
    if len(lei) != 20:
        return False
    numeric = _lei_letters_to_digits(lei)
    return int(numeric) % 97 == 1


print(lei_check_digits("HWUPKR0MPOU8FGXBT3"))  # -> 94
print(lei_validate("HWUPKR0MPOU8FGXBT394"))   # -> True  (Apple Inc.)
```

**Sources:** [ISO 17442-1:2020 sample PDF](https://cdn.standards.iteh.ai/samples/78829/91dbf00ce17b4b4293a06e5e1312c6a8/ISO-17442-1-2020.pdf) · [GLEIF — What is LEI](https://www.leiroc.org/lei.htm) · [Legal Entity Identifier — Wikipedia](https://en.wikipedia.org/wiki/Legal_Entity_Identifier) · [GLEIF LEI record for Apple Inc.](https://lei.bloomberg.com/leis/view/HWUPKR0MPOU8FGXBT394)

---

## FIGI

**Financial Instrument Global Identifier** — an OMG (Object Management Group) open standard, minted in practice mostly by Bloomberg as the registered "Certified Provider." Notable for being free and open (no licensing fee, unlike CUSIP), and for issuing a distinct FIGI **per venue** plus a "composite" FIGI that rolls up all US listings of one instrument.

### Structure

|Position|Length|Field|
|---|---|---|
|1–2|2|Certified Provider prefix (`BB` = Bloomberg)|
|3|1|Always `G` (Global Identifier)|
|4–11|8|Randomly assigned, alphanumeric, **vowels excluded**|
|12|1|Check digit|

Characters 4-11 are alphanumeric (less vowels), and the 12th digit is numeric, serving as a check digit calculated with the Modulus 10 Double Add Double technique. Apple Inc.'s common stock, for instance, trades on 14 exchanges in the United States, and there is a unique FIGI for the common stock on each individual exchange as well as a composite FIGI representing the company's common stock across all US exchanges.

### Real examples

|FIGI|Represents|
|---|---|
|`BBG000B9XRY4`|Apple Inc. common stock — US composite|
|`BBG000BLNQ16`|Reference example used in the python-stdnum test suite|

### Check-digit algorithm (Modulus 10 Double Add Double, FIGI variant)

The general description: the first 3 characters begin with 'xxG', positions 4-11 are alphanumeric excluding vowels, and the last digit is a check digit calculated with a variation of the Modulus 10 Double Add Double formula. The precise, code-verified version doubles every _second_ character counting from the left (index 1, 3, 5... in 0-based terms) rather than from the right as CUSIP does:

```python
_FIGI_ALPHABET = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"

def figi_check_digit(body11: str) -> int:
    """body11 = the first 11 characters of a FIGI (BBG + 8 alnum chars)."""
    total = 0
    for i, ch in enumerate(body11):
        v = _FIGI_ALPHABET.index(ch)
        if i % 2 == 1:                       # double every 2nd character (0-indexed)
            v *= 2
        total += sum(int(d) for d in str(v))  # split any 2-digit result and add
    return (10 - total % 10) % 10


def figi_validate(figi: str) -> bool:
    figi = figi.strip().upper()
    if len(figi) != 12:
        return False
    return figi_check_digit(figi[:11]) == int(figi[11])


print(figi_validate("BBG000B9XRY4"))   # -> True  (Apple Inc., US composite)
print(figi_validate("BBG000BLNQ16"))   # -> True
```

**Sources:** [OpenFIGI — Check-Digit Calculation](https://www.openfigi.com/insights/all/2018/6/15/figi-check-digit-calculation) · [OpenFIGI — Overview](https://www.openfigi.com/about/overview) · [python-stdnum figi.py](https://github.com/arthurdejong/python-stdnum/blob/master/stdnum/figi.py) · [Financial Instrument Global Identifier — Wikipedia](https://en.wikipedia.org/wiki/Financial_Instrument_Global_Identifier)

---

## Other identifiers worth knowing

|Identifier|What it is|Format|Notes|
|---|---|---|---|
|**CIK** (Central Index Key)|SEC EDGAR filer ID for companies and individuals|Up to 10 digits, zero-padded|CIKs are always numeric and stored as 10 digits with leading zeros, so 320193 and 0000320193 both refer to Apple Inc. Not a security identifier — it's a _filer_ identifier, one per legal entity, and it never changes even through renames or mergers|
|**Ticker symbol**|Exchange trading symbol (`AAPL`, `MSFT`)|1–5 letters, exchange-specific|Not globally unique — the same ticker can mean different companies on different exchanges, and tickers can be reassigned after a delisting|
|**WKN** (Wertpapierkennnummer)|Germany's legacy national security code|6 alphanumeric characters|Being phased out in favor of ISIN, but still quoted for German securities and directly embeds into German ISINs|
|**CINS**|International sibling of CUSIP for non-North-American issuers|9 characters, letter in position 1|See [[#CUSIP]] section above|

---

## Quick-reference decision tree

> [!question] "I have a security code — what is it?"
> 
> - **20 characters, alphanumeric** → [[#LEI]] (and it identifies the _entity_, not the security)
> - **12 characters, starts with 2 letters (country code)** → [[#ISIN]]
> - **12 characters, starts with `BBG`** → [[#FIGI]]
> - **9 characters, no country-code-looking prefix, from a US/CA context** → [[#CUSIP]]
> - **7 characters, no vowels** → [[#SEDOL]]

---

## Consolidated source list

- ISIN overview — [https://www.isin.com/isin/](https://www.isin.com/isin/)
- CUSIP overview — [https://www.isin.com/cusip/](https://www.isin.com/cusip/)
- SEC — What is a CUSIP — [https://www.sec.gov/answers/cusip](https://www.sec.gov/answers/cusip)
- CUSIP Global Services — About CGS Identifiers — [https://www.cusip.com/identifiers.html](https://www.cusip.com/identifiers.html)
- python-stdnum (reference implementations for CUSIP, SEDOL, FIGI) — [https://github.com/arthurdejong/python-stdnum](https://github.com/arthurdejong/python-stdnum)
- Apache Commons Validator — check-digit routines — [https://commons.apache.org/validator/apidocs/org/apache/commons/validator/routines/checkdigit/](https://commons.apache.org/validator/apidocs/org/apache/commons/validator/routines/checkdigit/)
- Luhn algorithm — [https://en.wikipedia.org/wiki/Luhn_algorithm](https://en.wikipedia.org/wiki/Luhn_algorithm)
- ISIN Checksum walkthrough — [https://fastercapital.com/content/ISIN-Checksum--Verifying-the-Validity-of-an-ISIN-Code.html](https://fastercapital.com/content/ISIN-Checksum--Verifying-the-Validity-of-an-ISIN-Code.html)
- CUSIP → ISIN converter (worked example) — [https://alltools.dev/tools/finance/cusip-to-isin-converter/](https://alltools.dev/tools/finance/cusip-to-isin-converter/)
- SEDOL — Wikipedia — [https://en.wikipedia.org/wiki/SEDOL](https://en.wikipedia.org/wiki/SEDOL)
- LEI — ISO 17442-1:2020 standard sample — [https://cdn.standards.iteh.ai/samples/78829/91dbf00ce17b4b4293a06e5e1312c6a8/ISO-17442-1-2020.pdf](https://cdn.standards.iteh.ai/samples/78829/91dbf00ce17b4b4293a06e5e1312c6a8/ISO-17442-1-2020.pdf)
- GLEIF (Global LEI Foundation) — [https://www.leiroc.org/lei.htm](https://www.leiroc.org/lei.htm)
- OpenFIGI — Check-Digit Calculation — [https://www.openfigi.com/insights/all/2018/6/15/figi-check-digit-calculation](https://www.openfigi.com/insights/all/2018/6/15/figi-check-digit-calculation)
- FIGI — Wikipedia — [https://en.wikipedia.org/wiki/Financial_Instrument_Global_Identifier](https://en.wikipedia.org/wiki/Financial_Instrument_Global_Identifier)
- CIK — SEC EDGAR — [https://www.sec.gov/edgar/searchedgar/cik.htm](https://www.sec.gov/edgar/searchedgar/cik.htm)

> [!note] All Python snippets in this note were executed and validated against the real-world examples shown (Apple, Microsoft, Alphabet, BAE Systems) before being included.