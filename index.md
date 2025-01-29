# TallyPrime XML API Documentation

## Table of Contents

- [Prerequisites](#prerequisites)
- [Base Url & Headers](#baseurl)
- [Accounting Masters](#accounting-masters)
  - [Groups](#groups)
  - [Ledgers](#ledgers)
- [Inventory Masters](#inventory-masters)
  - [Stock Groups](#stock-groups)
  - [Stock Items](#stock-items)
  - [Units of Measure](#units-of-measure)
  - [Godowns](#godowns)
- [Transactions](#transactions)
  - [Sales](#sales)
  - [Alter/Cancel](#altercancel)
- [Reports](#reports)
  - [Standard Reports](#standard-reports)
  - [Custom Reports](#custom-reports)
- [Errors & Best Practices](#errors--best-practices)

---

## Prerequisites <a name="prerequisites"></a>

1. **Configure Tally**:

   - Enable Data Synchronization: `F12 > Configure > Data Synchronization > Set as Server (Port 9000)`.
   - Load your company: `Company > Select`.

2. **Dependencies**:
   - Create masters (Groups/Ledgers/Stock Items) before transactions.
   - Enable features like batches/godowns if required.

---

## Base URL & Headers <a name="baseurl"></a>

URL: http://localhost:9000 (Default port: 9000).

Headers:

```http
Content-Type: text/xml
```

---

## Accounting Masters <a name="accounting-masters"></a>

### Groups <a name="groups"></a>

#### Create a Group

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <GROUP Action="Create">
            <NAME>North Zone Debtors</NAME>
            <PARENT>Sundry Debtors</PARENT>
          </GROUP>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

### Ledgers <a name="ledgers"></a>

#### Create a Ledger

```xml
Copy
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <LEDGER Action="Create">
            <NAME>Customer ABC</NAME>
            <PARENT>North Zone Debtors</PARENT>
          </LEDGER>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

#### Add Address to Ledger

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <LEDGER NAME="Customer ABC" Action="Alter">
            <ADDRESS.LIST TYPE="String">
              <ADDRESS>123 Business Lane</ADDRESS>
            </ADDRESS.LIST>
          </LEDGER>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

## Inventory Masters <a name="inventory-masters"></a>

### Stock Groups <a name="stock-groups"></a>

#### Create a Stock Group

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <STOCKGROUP Action="Create">
            <NAME>Electronics</NAME>
          </STOCKGROUP>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

### Stock Items <a name="stock-items"></a>

#### Create a Stock Item

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <STOCKITEM Action="Create">
            <NAME>Blue Jeans</NAME>
            <BASEUNITS>Nos</BASEUNITS>
            <PARENT>Apparel</PARENT>
          </STOCKITEM>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

#### Add Alias to Stock Item

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <STOCKITEM NAME="Blue Jeans" Action="Alter">
            <NAME.LIST TYPE="String">
              <NAME>Denim Jeans</NAME>
            </NAME.LIST>
          </STOCKITEM>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

### Units of Measure <a name="units-of-measure"></a>

#### Create a Simple Unit

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <UNIT Action="Create">
            <NAME>Nos</NAME>
            <ISSIMPLEUNIT>Yes</ISSIMPLEUNIT>
          </UNIT>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

#### Create a Compound Unit

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <UNIT Action="Create">
            <NAME>Box of 10 Pcs</NAME>
            <ISSIMPLEUNIT>No</ISSIMPLEUNIT>
            <BASEUNITS>Box</BASEUNITS>
            <ADDITIONALUNITS>Pcs</ADDITIONALUNITS>
            <CONVERSION>10</CONVERSION>
          </UNIT>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

### Godowns <a name="godowns"></a>

#### Create a Godown

```xml
<ENVELOPE>
  <HEADER>
    <TALLYREQUEST>Import Data</TALLYREQUEST>
  </HEADER>
  <BODY>
    <IMPORTDATA>
      <REQUESTDESC>
        <REPORTNAME>All Masters</REPORTNAME>
      </REQUESTDESC>
      <REQUESTDATA>
        <TALLYMESSAGE>
          <GODOWN Action="Create">
            <NAME>Warehouse A</NAME>
          </GODOWN>
        </TALLYMESSAGE>
      </REQUESTDATA>
    </IMPORTDATA>
  </BODY>
</ENVELOPE>
```

## Transactions <a name="transactions"></a>

### Sales <a name="sales"></a>

#### Sales Voucher (Accounting View)

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Import</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Vouchers</ID>
  </HEADER>
  <BODY>
    <DATA>
      <TALLYMESSAGE>
        <VOUCHER>
          <DATE>20231015</DATE>
          <VOUCHERTYPENAME>Sales</VOUCHERTYPENAME>
          <LEDGERENTRIES.LIST>
            <LEDGERNAME>Customer ABC</LEDGERNAME>
            <ISDEEMEDPOSITIVE>Yes</ISDEEMEDPOSITIVE>
            <AMOUNT>-15000.00</AMOUNT>
          </LEDGERENTRIES.LIST>
          <ALLINVENTORYENTRIES.LIST>
            <STOCKITEMNAME>Blue Jeans</STOCKITEMNAME>
            <RATE>1500.00/Nos</RATE>
            <ACTUALQTY>10 Nos</ACTUALQTY>
          </ALLINVENTORYENTRIES.LIST>
        </VOUCHER>
      </TALLYMESSAGE>
    </DATA>
  </BODY>
</ENVELOPE>
```

#### Sales Invoice (Invoice View)

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Import</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Vouchers</ID>
  </HEADER>
  <BODY>
    <DATA>
      <TALLYMESSAGE>
        <VOUCHER>
          <DATE>20231015</DATE>
          <VOUCHERTYPENAME>Sales</VOUCHERTYPENAME>
          <PERSISTEDVIEW>Invoice Voucher View</PERSISTEDVIEW>
          <ISINVOICE>Yes</ISINVOICE>
          <LEDGERENTRIES.LIST>
            <LEDGERNAME>Customer ABC</LEDGERNAME>
            <AMOUNT>-15000.00</AMOUNT>
          </LEDGERENTRIES.LIST>
        </VOUCHER>
      </TALLYMESSAGE>
    </DATA>
  </BODY>
</ENVELOPE>
```

### Alter/Cancel <a name="altercancel"></a>

#### Alter a Voucher

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Import</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Vouchers</ID>
  </HEADER>
  <BODY>
    <DATA>
      <TALLYMESSAGE>
        <VOUCHER DATE="20231015" TAGNAME="VoucherNumber" TAGVALUE="1001" Action="Alter" VCHTYPE="Sales">
          <NARRATION>Updated sales details</NARRATION>
        </VOUCHER>
      </TALLYMESSAGE>
    </DATA>
  </BODY>
</ENVELOPE>
```

#### Cancel a Voucher

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Import</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Vouchers</ID>
  </HEADER>
  <BODY>
    <DATA>
      <TALLYMESSAGE>
        <VOUCHER DATE="20231015" TAGNAME="VoucherNumber" TAGVALUE="1001" Action="Cancel" VCHTYPE="Sales"/>
      </TALLYMESSAGE>
    </DATA>
  </BODY>
</ENVELOPE>
```

## Reports <a name="reports"></a>

### Standard Reports <a name="standard-reports"></a>

#### Trial Balance

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Export</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Trial Balance</ID>
  </HEADER>
  <BODY>
    <DESC>
      <STATICVARIABLES>
        <SVEXPORTFORMAT>$$SysName:XML</SVEXPORTFORMAT>
      </STATICVARIABLES>
    </DESC>
  </BODY>
</ENVELOPE>
```

#### Day Book (Filtered by Date)

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Export</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>DayBook</ID>
  </HEADER>
  <BODY>
    <DESC>
      <STATICVARIABLES>
        <SVEXPORTFORMAT>$$SysName:XML</SVEXPORTFORMAT>
        <SVFROMDATE>20231001</SVFROMDATE>
        <SVTODATE>20231031</SVTODATE>
      </STATICVARIABLES>
    </DESC>
  </BODY>
</ENVELOPE>
```

### Custom Reports <a name="custom-reports"></a>

#### Simple Trial Balance (TDL)

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Export</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Simple Trial Balance</ID>
  </HEADER>
  <BODY>
    <DESC>
      <STATICVARIABLES>
        <SVEXPORTFORMAT>$$SysName:XML</SVEXPORTFORMAT>
      </STATICVARIABLES>
      <TDL>
        <TDLMESSAGE>
          <REPORT NAME="Simple Trial Balance">
            <FORMS>Simple TB Form</FORMS>
          </REPORT>
          <FORM NAME="Simple TB Form">
            <TOPPARTS>Simple TB Part</TOPPARTS>
          </FORM>
          <PART NAME="Simple TB Part">
            <REPEAT>Simple TB Details : Ledgers</REPEAT>
          </PART>
          <COLLECTION NAME="Ledgers">
            <TYPE>Ledger</TYPE>
          </COLLECTION>
        </TDLMESSAGE>
      </TDL>
    </DESC>
  </BODY>
</ENVELOPE>
```

### HTML Format Report

```xml
<ENVELOPE>
  <HEADER>
    <VERSION>1</VERSION>
    <TALLYREQUEST>Export</TALLYREQUEST>
    <TYPE>Data</TYPE>
    <ID>Trial Balance</ID>
  </HEADER>
  <BODY>
    <DESC>
      <STATICVARIABLES>
        <SVEXPORTFORMAT>$$SysName:HTML</SVEXPORTFORMAT>
      </STATICVARIABLES>
    </DESC>
  </BODY>
</ENVELOPE>
```

## Errors & Best Practices <a name="errors--best-practices"></a>

| Error                       | Solution                       |
| --------------------------- | ------------------------------ |
| Ledger not found            | Create the ledger first.       |
| Stock Item not found        | Ensure the stock item exists.  |
| Invalid date format         | Use YYYYMMDD (e.g., 20231015). |
| Voucher totals do not match | Ensure Debit = Credit.         |

### Best Practices:

- Validate XML with tools like TallyPrime Developer.
- Check Tally.imp logs for errors.
- Use `<SVFROMDATE>` and `<SVTODATE>` for incremental data fetches.
