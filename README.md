[![image](https://github.com/engenere/BrazilFiscalReport/workflows/tests/badge.svg)](https://github.com/Engenere/BrazilFiscalReport/actions)
[![image](https://codecov.io/gh/engenere/BrazilFiscalReport/branch/main/graph/badge.svg)](https://app.codecov.io/gh/Engenere/BrazilFiscalReport)
[![image](https://img.shields.io/github/languages/top/Engenere/brazilfiscalreport)](https://pypi.org/project/BrazilFiscalReport/)
[![image](https://img.shields.io/pypi/v/brazilfiscalreport.svg)](https://pypi.org/project/BrazilFiscalReport/)
[![image](https://img.shields.io/pypi/l/brazilfiscalreport)](https://github.com/Engenere/BrazilFiscalReport/blob/main/LICENSE)
![beta](https://img.shields.io/badge/status-beta-orange)
# Brazil Fiscal Report

Python library for generating Brazilian auxiliary fiscal documents in PDF from XML documents.

## Supported Documents 📄

- DANFE - Documento Auxiliar da Nota Fiscal Eletrônica (NF-e)
- DACCe - Documento Auxiliar da Carta de Correção Eletrônica (CC-e )
- DACTE - Documento Auxiliar do Conhecimento de Transporte Eletrônico (CT-e)
- DAMDFE - Documento Auxiliar do Manifesto Eletrônico de Documentos Fiscais (MDF-e)

## Beta Stage Notice 🚧

This library is currently in the beta stage of development. While it has many of the intended features implemented, it is still undergoing testing and improvements. Users should note that during this phase, functionality may change and some instability may occur. We welcome feedback on any issues or suggestions for enhancements. Use in production environments should be approached with caution.

## Dependencies 🛠️

- [FPDF2](https://github.com/py-pdf/fpdf2) - PDF creation library for Python
- phonenumbers
- python-barcode
- qrcode (only required for DACTE)

## To install 🔧

```bash
pip install brazilfiscalreport
```

### Installing DACTE with Dependencies
If you specifically need the DACTE functionality, you can install it along with its required dependencies using:

```bash
pip install brazilfiscalreport[dacte]
```

## Usage examples 🚀
### DANFE

```python
from brazilfiscalreport.danfe import Danfe

# Path to the XML file
xml_file_path = 'nfe.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Instantiate the DANFE object with the loaded XML content
danfe = Danfe(xml=xml_content)

# Save the generated PDF to a file
danfe.output('danfe.pdf')
```
### DACCe

```python
from brazilfiscalreport.dacce import DaCCe

# Path to the XML file
xml_file_path = 'cce.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Instantiate the CC-e PDF object with the loaded XML content
cce = DaCCe(xml=xml_content)

# Save the generated PDF to a file
cce.output('cce.pdf')
```

### DACTE

```python
from brazilfiscalreport.dacte import Dacte

# Path to the XML file
xml_file_path = 'dacte.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Instantiate the DACTE object with the loaded XML content
dacte = Dacte(xml=xml_content)

# Save the generated PDF to a file
dacte.output('dacte.pdf')
```

### DAMDFE

```python
from brazilfiscalreport.damdfe import Damdfe

# Path to the XML file
xml_file_path = 'damdfe.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Instantiate the DAMDFE object with the loaded XML content
damdfe = Damdfe(xml=xml_content)

# Save the generated PDF to a file
damdfe.output('damdfe.pdf')
```

## Samples 📝

Some sample PDFs generated by our unit tests are available for viewing in the [tests/generated](https://github.com/Engenere/BrazilFiscalReport/tree/main/tests/generated) directory.

## Customizing DANFE 🎨

This section describes how to customize the PDF output of the DANFE using the ``DanfeConfig`` class. You can adjust various settings such as margins, fonts, and tax configurations according to your needs.

###  Configuration Options ⚙️

Here is a breakdown of all the configuration options available in ``DanfeConfig``:

1. **Logo**
   - **Type**: ``str``, ``BytesIO``, or ``bytes``
   - **Description**: Path to the logo file or binary image data to be included in the PDF. You can use a file path string or pass image data directly.
   - **Example**:
        ```python
       config.logo = "path/to/logo.jpg"  # Using a file path
      ```
   - **Default**: No logo.


2. **Margins**
   - **Type**: ``Margins``
   - **Fields**: ``top``, ``right``, ``bottom``, ``left`` (all of type ``Number``)
   - **Description**: Sets the page margins for the PDF document.
   - **Example**:
        ```python
        config.margins = Margins(top=5, right=5, bottom=5, left=5)
        ```
   - **Default**: top, right, bottom and left is 5 mm.

3. **Receipt Position**
   - **Type**: ``ReceiptPosition`` (Enum)
   - **Values**: ``TOP``, ``BOTTOM``, ``LEFT``
   - **Description**: Position of the receipt section in the DANFE.
   - **Example**:
        ```python
        config.receipt_pos = ReceiptPosition.BOTTOM
        ```
   - **Default**: ``TOP`` when portrait, ``LEFT`` when landscape orientation.
   - **Note**: In landscape orientation, the receipt position is far left; customization is not permitted.

4. **Decimal Configuration**
   - **Type**: ``DecimalConfig``
   - **Fields**: ``price_precision``, ``quantity_precision`` (both ``int``)
   - **Description**: Defines the number of decimal places for prices and quantities.
   - **Example**:
        ```python
       config.decimal_config = DecimalConfig(price_precision=2, quantity_precision=3)
        ```
   - **Default**: 4

5. **Tax Configuration**
   - **Type**: ``TaxConfiguration`` (Enum)
   - **Values**: ``STANDARD_ICMS_IPI``, ``ICMS_ST_ONLY``, ``WITHOUT_IPI``
   - **Description**: Specifies the tax fields to be displayed.
   - **Example**:
        ```python
        config.tax_configuration = TaxConfiguration.WITHOUT_IPI
        ```
   - **Default**: ``STANDARD_ICMS_IPI``
   - **WARNING: This feature is not yet implemented.**

6. **Invoice Display**
   - **Type**: ``InvoiceDisplay`` (Enum)
   - **Values**: ``DUPLICATES_ONLY``, ``FULL_DETAILS``
   - **Description**: Controls the detail level in the invoice section of the DANFE.
   - **Example**::
        ```python
       config.invoice_display = InvoiceDisplay.DUPLICATES_ONLY
        ```
   - **Default**: ``FULL_DETAILS``

7. **Font Type**
   - **Type**: ``FontType`` (Enum)
   - **Values**: ``COURIER``, ``TIMES``
   - **Description**: Font style used throughout the PDF document.
   - **Example**::
        ```python
       config.font_type = FontType.COURIER
        ```
   - **Default**: ``TIMES``

8. **Display PIS COFINS**
   - **Type**: ``Bool``
   - **Values**: ``True``, ``False``
   - **Description**: Displays PIS and COFINS taxes in the DANFE totals.
   - **Example**::
        ```python
       config.display_pis_cofins = True
        ```
   - **Default**: ``False``
### Usage Example with Customization

Here’s how to set up a ``DanfeConfig`` object with a full set of customizations::

```python
from brazilfiscalreport.danfe import (
    Danfe,
    DanfeConfig,
    DecimalConfig,
    FontType,
    InvoiceDisplay,
    Margins,
    ReceiptPosition,
    TaxConfiguration,
)

# Path to the XML file
xml_file_path = 'nfe.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Create a configuration instance
config = DanfeConfig(
    logo='path/to/logo.png',
    margins=Margins(top=10, right=10, bottom=10, left=10),
    receipt_pos=ReceiptPosition.BOTTOM,
    decimal_config=DecimalConfig(price_precision=2, quantity_precision=2),
    tax_configuration=TaxConfiguration.ICMS_ST,
    invoice_display=InvoiceDisplay.FULL_DETAILS,
    font_type=FontType.TIMES
)

# Use this config when creating a Danfe instance
danfe = Danfe(xml_content, config=config)
danfe.output('output_danfe.pdf')
```

## Customizing DACTE

This section describes how to customize the PDF output of the DACTE using the DacteConfig class. You can adjust various settings such as margins, fonts, and tax configurations according to your needs.

###  Configuration Options

Here is a breakdown of all the configuration options available in ``DanfeConfig``:

1. **Logo**
   - **Type**: ``str``, ``BytesIO``, or ``bytes``
   - **Description**: Path to the logo file or binary image data to be included in the PDF. You can use a file path string or pass image data directly.
   - **Example**:
        ```python
       config.logo = "path/to/logo.jpg"  # Using a file path
      ```
   - **Default**: No logo.


2. **Margins**
   - **Type**: ``Margins``
   - **Fields**: ``top``, ``right``, ``bottom``, ``left`` (all of type ``Number``)
   - **Description**: Sets the page margins for the PDF document.
   - **Example**:
        ```python
        config.margins = Margins(top=5, right=5, bottom=5, left=5)
        ```
   - **Default**: top, right, bottom and left is 5 mm.

3. **Font Type**
   - **Type**: ``FontType`` (Enum)
   - **Values**: ``COURIER``, ``TIMES``
   - **Description**: Font style used throughout the PDF document.
   - **Example**::
        ```python
       config.font_type = FontType.COURIER
        ```
   - **Default**: ``TIMES``

### Usage Example with Customization

Here’s how to set up a DacteConfig object with a full set of customizations:

```python
from brazilfiscalreport.dacte import (
    Dacte,
    DacteConfig,
    FontType,
    Margins,
)

# Path to the XML file
xml_file_path = 'cte.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Create a configuration instance
config = DacteConfig(
    logo='path/to/logo.png',
    margins=Margins(top=10, right=10, bottom=10, left=10),
    font_type=FontType.TIMES
)

# Use this config when creating a Dacte instance
dacte = Dacte(xml_content, config=config)
dacte.output('output_dacte.pdf')
```

## Customizing DAMDFE

This section describes how to customize the PDF output of the DAMDFE using the DamdfeConfig class. You can adjust various settings such as margins, fonts, and tax configurations according to your needs.

###  Configuration Options

Here is a breakdown of all the configuration options available in ``DamdfeConfig``:

1. **Logo**
   - **Type**: ``str``, ``BytesIO``, or ``bytes``
   - **Description**: Path to the logo file or binary image data to be included in the PDF. You can use a file path string or pass image data directly.
   - **Example**:
        ```python
       config.logo = "path/to/logo.jpg"  # Using a file path
      ```
   - **Default**: No logo.


2. **Margins**
   - **Type**: ``Margins``
   - **Fields**: ``top``, ``right``, ``bottom``, ``left`` (all of type ``Number``)
   - **Description**: Sets the page margins for the PDF document.
   - **Example**:
        ```python
        config.margins = Margins(top=10, right=10, bottom=10, left=10)
        ```
   - **Default**: top, right, bottom and left is 5 mm.

3. **Font Type**
   - **Type**: ``FontType`` (Enum)
   - **Values**: ``COURIER``, ``TIMES``
   - **Description**: Font style used throughout the PDF document.
   - **Example**::
        ```python
       config.font_type = FontType.COURIER
        ```
   - **Default**: ``TIMES``

### Usage Example with Customization

Here’s how to set up a DamdfeConfig object with a full set of customizations:

```python
from brazilfiscalreport.damdfe import (
    Damdfe,
    DacteConfig,
    FontType,
    Margins,
)

# Path to the XML file
xml_file_path = 'mdf-e.xml'

# Load XML Content
with open(xml_file_path, "r", encoding="utf8") as file:
    xml_content = file.read()

# Create a configuration instance
config = DamdfeConfig(
    logo='path/to/logo.png',
    margins=Margins(top=10, right=10, bottom=10, left=10),
    font_type=FontType.TIMES
)

# Use this config when creating a Damdfe instance
damdfe = Damdfe(xml_content, config=config)
damdfe.output('output_dacte.pdf')
```

## Credits 🙌
This is a fork of the [nfe_utils](https://github.com/edsonbernar/nfe_utils) project, originally created by [Edson Bernardino](https://github.com/edsonbernar).

## Maintainer 🛠️
[![Engenere](https://storage.googleapis.com/eng-imagens/logo-fundo-preto.webp)]([#](https://engenere.one/))
