
# Google Sheets OpenAI API Integration

## Introduction

This is an into to Google Sheets OpenAI API Integration script. This tool is designed to help you analyse unstructured text, such as customer reviews and NPS feedback, directly within Google Sheets using the OpenAI API. This script allows you to automate text analysis and ensure adherence to a predefined schema, making it easier to extract structured information from your unstructured data in a conveinet way.

### Key Functionalities

- **LLM-based Text Analysis**: Automatically analyse and categorise unstructured text.
- **Customisable Schema**: Define your own schema in Google Sheets to ensure consistent data structure.
- **CSV and JSON Parsing**: Handle responses in both CSV and JSON formats.
- **Data Validation**: Ensure that the analysed data adheres to the expected schema.
- **Use Your Own OpenAI**: Data only passes to OpenAI API and no other external service.

### Use Cases

- **Customer Feedback Analysis**: Quickly analyse customer reviews to understand common issues and sentiments.
- **Trend Identification**: Identify trends in customer feedback to improve products or services.
- **Automated Reporting**: Generate reports based on analysed text data for stakeholders.

## Getting Started

This section will guide you through the initial setup required to use the Google Sheets OpenAI API Integration. Follow these steps to prepare your environment and get the script running.

### Prerequisites

Before you begin, ensure you have the following:

- **Google Account**: You need a Google account to access Google Sheets and Google Apps Script.
- **Google Sheets**: Create a new Google Sheet or use an existing one where you will store your data and schema.
- **OpenAI API Key**: Use your own API key from OpenAI.

### Setting Up the Environment

#### 1. Creating a New Google Apps Script Project

1. **Open your Google Sheets document.**
2. **Click on `Extensions` > `Apps Script` to open the script editor.**
3. **Delete any existing code in the script editor.**
4. **Copy the script from the provided script file and paste it into the script editor.**
5. **Save the script** by clicking the disk icon or pressing `Ctrl+S` (Windows) or `Cmd+S` (Mac).
6. **Authorize the script**: The first time you run the script, you will be prompted to authorize it. Follow the on-screen instructions to grant the necessary permissions.
7. **Run the script**: Select the function `callOpenAI` from the dropdown menu and click the play button to execute the script.

### Setting the OpenAI API Key

1. **Open the Google Apps Script editor** in your Google Sheets document.
2. **Click on `File` in the top menu.**
3. **Select `Project properties` from the dropdown menu.**
4. **Go to the `Script Properties` tab.**
5. **Click `+ Add row`.**
6. **Set the `Name` to `OPENAI_API_KEY`.**
7. **Set the `Value` to your OpenAI API key.**
8. **Click `Save`.**

#### 2. Setting Up the Google Sheets

You will need two sheets in your Google Sheets document:

1. **SystemPrompts Sheet**: This sheet will contain the mapping between parameter names and their corresponding model, system prompt, schema sheet, and response format.
2. **Schema Sheet**: This sheet will define the structure of the data you expect to receive from the OpenAI API.

##### SystemPrompts Sheet Structure

| Parameter Name | Model         | System Prompt                           | Schema Sheet | Response Format |
|----------------|---------------|-----------------------------------------|--------------|-----------------|
| TextAnalysis   | gpt-3.5-turbo | [Your system prompt as described below] | DataSchema   | CSV             |
| TextAnalysis   | gpt-4o        | [Your system prompt as described below] | DataSchema   | JSON            |

##### Schema Sheet Structure

**Schema Sheet (DataSchema) Example:**

| Property Name    | Property Type | Property Description                                  |
|------------------|---------------|-------------------------------------------------------|
| Category         | string        | The main category of the data                         |
| Subcategory1     | string        | The first subcategory of the data                     |
| Subcategory2     | string        | The second subcategory of the data (if applicable)    |

Ensure that these sheets are set up correctly in your Google Sheets document before proceeding to add the script. Please ensure the names are exactly as written here to ensure cosistency with the script.

### Customising the System Prompt

The system prompt guides the OpenAI model in analysing the text. You can customise this prompt directly in the SystemPrompts sheet.

#### Example System Prompt

```markdown
You are a data analyst who assigns categories and subcategories to the given data. The goal is to accurately tag data based on predefined categories to identify trends and areas for improvement. Take the input data and assign one category and a maximum of two subcategories.

Categories:
- Category1
  - Subcategory1.1
  - Subcategory1.2
- Category2
  - Subcategory2.1
  - Subcategory2.2

Only output the category and subcategories. For example: Category1; Subcategory1.1; Subcategory1.2
```

### Example Output

For the given reviews, the output might look like this:

| Customer Reviews                                   | Category            | Subcategory1            | Subcategory2          |
|----------------------------------------------------|---------------------|-------------------------|-----------------------|
| The app keeps crashing on my phone.                | App Functionality   | Errors and Bugs         |                       |
| I love the new update, but the navigation is a bit confusing. | App Functionality   | Updates                 | Navigation            |
| Excellent customer service, but delivery was late. | Customer Service    | Problem Resolution      | Delivery Delays       |

### Updating the System Prompt in the SystemPrompts Sheet

1. **Open your Google Sheets document.**
2. **Navigate to the `SystemPrompts` sheet.**
3. **Find the row corresponding to the Parameter Name you are using.**
4. **Update the `System Prompt` column with your customised prompt.

## Running the Script

To use the script within Google Sheets, you can call the `callOpenAI` function from a custom menu or directly from a cell.

#### Using the Script Directly from a Cell

**Use the function in a cell** in your Google Sheets document:

```
==callOpenAI("TextAnalysis", A1)
```

Replace `A1` with the reference to the cell containing the input data.
Replace "TextAnalysis" with the Parameter Name of the function you are using in 'SystemPrompts' (column A)

## Troubleshooting

This section provides solutions to common issues you might encounter while using the Google Sheets Classifier. Follow these steps to resolve any problems and ensure smooth operation.

### Common Issues and Solutions

#### Issue: "After I open the spreadsheet API calls a repeated"
- **Solution**: This is a known issue that cannot be easily solved. The recommendation is to copy-paste the outputs, store them as values and remove formulas to avoid repeated API calls.
- 
#### Issue: "Schema sheet not found: undefined"
- **Solution**: Ensure that the `System Prompts` sheet contains the correct schema sheet name for the parameter you are using. Verify that the schema sheet name is spelled correctly and exists in your Google Sheets document.

#### Issue: "OpenAI response does not contain the expected message content"
- **Solution**: Check the system prompt to ensure it is clear and concise. Verify that the OpenAI API is returning valid responses by testing it with sample inputs directly in the OpenAI API playground.

#### Issue: "Missing required property"
- **Solution**: Ensure that the schema defined in your schema sheet matches the expected properties in the OpenAI API response. Check for typos or missing properties in both the schema sheet and the system prompt.

#### Issue: "Invalid data type for property"
- **Solution**: Verify that the data types specified in the schema sheet match the data types returned by the OpenAI API. Update the schema sheet to correct any mismatched data types.


### Frequently Asked Questions (FAQs)

#### Q: Where does my data go?
**A:** You are using your own OpenAI API key and any data you adding via the prompts will be sent to OpenAI. It does not get routed via any other service.

#### Q: How do I update the system prompt?
**A:** Open your Google Sheets document, navigate to the `System Prompts` sheet, find the row corresponding to the parameter name you are using, and update the `System Prompt` column with your customized prompt.

#### Q: Can I use multiple schemas in one Google Sheets document?
**A:** Yes, you can define multiple schemas in different sheets and reference them in the `System Prompts` sheet. Ensure that the correct schema sheet name is specified for each parameter.

#### Q: How do I change the response format?
**A:** Update the `Response Format` column in the `System Prompts` sheet to either `CSV` or `JSON`, depending on your preference.
