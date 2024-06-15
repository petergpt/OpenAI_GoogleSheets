
# Google Sheets OpenAI API Integration

## Introduction

This is an introduction to Google Sheets OpenAI API Integration script. This tool is designed to help you analyse unstructured text, such as customer reviews and NPS feedback, directly within Google Sheets using the OpenAI API. This script allows you to automate text analysis, making it easier to extract structured information from your unstructured data in a convenient way.

### Key Functionalities

- **LLM-based Text Analysis**: Automatically analyse and categorise unstructured text.
- **One-button revised call**: A button to send a second call to improve the output.
- **Use Your Own OpenAI API Key**: Data only passes to OpenAI API and no other external service.

## Getting Started

This section will guide you through the initial setup required to use the Google Sheets OpenAI API Integration. Follow these steps to prepare your environment and get the script running.

### Prerequisites

Before you begin, ensure you have the following:

- **Google Account**: You need a Google account to access Google Sheets and Google Apps Script.
- **Google Sheets**: Create a new Google Sheet or use an existing one where you will store your data.
- **OpenAI API Key**: Use your own API key from OpenAI.

### Setting Up the Environment

#### 1. Creating a New Google Apps Script Project

1. **Open your Google Sheets document.**
2. **Click on `Extensions` > `Apps Script` to open the script editor.**
3. **Delete any existing code in the script editor.**
4. **Copy the script from the provided script file and paste it into the script editor.**
5. **Save the script** by by clicking the 'Save' icon.
6. **Authorize the script**: The first time you run the script, you will be prompted to authorize it. Follow the on-screen instructions to grant the necessary permissions.
7. **Run the script**: Select the function `callOpenAI` from the dropdown menu and click the play button to execute the script.

### Setting the OpenAI API Key

1. ***Get your OpenAI API Key*** from https://platform.openai.com/api-keys
2. **Open the Google Apps Script editor** in your Google Sheets document.
3. **Click on `File` in the top menu.**
4. **Select `Project properties` from the dropdown menu.**
5. **Go to the `Script Properties` tab.**
6. **Click `+ Add row`.**
7. **Set the `Name` to `OPENAI_API_KEY`.**
8. **Set the `Value` to your OpenAI API key.**
9. **Click `Save`.**

#### 2. Setting Up the Google Sheets

You will need a sheet in your Google Sheets document called `SystemPrompts`:

##### SystemPrompts Sheet Structure

| Parameter Name | Model | System Prompt |
|----------------|-------|---------------|
| Review-4o      | gpt-4 | [Your system prompt as described below] |

### Customising the System Prompt

The system prompt guides the OpenAI model in analysing the text. You can customise this prompt directly in the `SystemPrompts` sheet.

#### Example System Prompt

```markdown
Given a customer review for an online retailer, extract and structure the relevant information into a CSV format without including the column headers. If any data is missing, specify it as 'null'. Use semicolons (;) as the delimiter. The structured output should include the following aspects in the given order:

Classification: Positive, Neutral, Negative
Summary: Summarise the customer's feedback in one sentence.
Issue: List any problems or complaints mentioned.
Positives: Highlight any positive aspects mentioned.
Sentiment: Overall sentiment expressed (Positive, Neutral, Negative).
Detailed Sentiment: More detailed emotions conveyed.
Products: Mention any specific products or services highlighted in the feedback.
Location: Any geographical locations mentioned.
NPS: Estimate a Net Promoter Score from 1 (least likely to recommend) to 10 (most likely to recommend).

The CSV output should use semicolons as delimiters and look like this:
Classification;Summary;Issue;Positives;Sentiment;Detailed Sentiment;Products;Location;NPS
```

### Example Output

For the given reviews, the output might look like this:

| Customer Reviews                                   | Structured Output                                                                                       |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| The app keeps crashing on my phone.                | Negative;The app keeps crashing on my phone.;Errors;null;Negative;Frustration;App;null;2               |
| I love the new update, but the navigation is a bit confusing. | Positive;I love the new update, but the navigation is a bit confusing.;Navigation;Updates;Positive;Mixed feelings;App;null;7 |
| Excellent customer service, but delivery was late. | Positive;Excellent customer service, but delivery was late.;Delivery;Customer service;Positive;Mixed;Service;null;5  |

## Running the Script

To use the script within Google Sheets, you can call the `callOpenAI` function directly from a cell.

#### Using the Script Directly from a Cell

**Use the function in a cell** in your Google Sheets document:

```
=callOpenAI("Review-4o", A1)
```

Replace `A1` with the reference to the cell containing the input data.
Replace "Review-4o" with your Parameter Name (column A) in SystemPrompts sheet.

### Revise API Response - Custom Button

The script also creates a custom button called 'Revise API Responses'. It allows you to select the cells for which you are not happy with the response and get the LLM to improve the response in a second call.

#### How to use it

1. **Select cells with bad responses** select as many cells as needed
2. **Click button** 'Revise API Responses' drop down, then select 'Re-send API request to OpenAI' button.
3. **API request** This will trigger a script to execute a second API call with an updated prompt that asks the same model to revise the output
4. **Updated response** The cell will now be updated with a response value from the LLM

## Troubleshooting

This section provides solutions to common issues you might encounter while using the Google Sheets OpenAI API Integration. Follow these steps to resolve any problems and ensure smooth operation.

### Common Issues and Solutions

#### Issue: "Script function not found: triggerApiCall"
- **Solution**: Ensure that all function names are correctly referenced in the script. Replace any outdated function names with the correct ones.

#### Issue: "OpenAI response does not contain the expected message content"
- **Solution**: Check the system prompt to ensure it is clear and concise. Verify that the OpenAI API is returning valid responses by testing it with sample inputs directly in the OpenAI API playground.

#### Issue: "Resive API Response button not working"
- **Solution**: Open the script, run the onOpen function, refresh the spreadsheet.

### Frequently Asked Questions (FAQs)

#### Q: Where does my data go?
**A:** You are using your own OpenAI API key and any data you add via the prompts will be sent to OpenAI. It does not get routed via any other service.

#### Q: How do I update the system prompt?
**A:** Open your Google Sheets document, navigate to the `System Prompts` sheet, find the row corresponding to the parameter name you are using, and update the `System Prompt` column with your customised prompt.

#### Q: Why do my API calls get re-sent when I open the spreadsheet?
**A:** It is a known issue that is a limitation of this integration. It is best to make the call, copy out the data and remove the formulas (leave one for future reference) that make the API calls to avoid unecessary calls.
