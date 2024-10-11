---
description: >-
  This Zapier integration will test a website's sustainability and provide
  scores and recommendations for improvement.
cover: ../.gitbook/assets/_40.jpg
coverY: 0
---

# Zapier Integration for Website Sustainability Testing

Trigger:

* App: Webhooks by Zapier
* Event: Catch Hook

Action:

* App: Custom Code by Zapier
* Code:

```
import requests

# Get website URL from trigger data
website_url = trigger_data["url"]

# Define API endpoint for sustainability testing
api_endpoint = "https://api.example.com/sustainability"

# Send request to API with website URL
response = requests.get(api_endpoint, params={"url": website_url})

# Check for successful response
if response.status_code == 200:
    # Parse JSON response
    data = response.json()

    # Extract sustainability scores and recommendations
    sustainability_score = data["score"]
    recommendations = data["recommendations"]

    # Create output message with scores and recommendations
    output_message = f"Sustainability Score: {sustainability_score}\n\nRecommendations:\n"
    for recommendation in recommendations:
        output_message += f"- {recommendation}\n"

    # Set output data
    return {"message": output_message}
else:
    # Handle error
    return {"error": "API request failed"}

```

The generated code may be subject to license restrictions that are not shown here. Use code with care. [Learn more ](https://cloud.google.com/duet-ai/docs/discover/code-generation-source-citation#how\_helps\_with\_coding)

### Explanation:

1. The trigger uses the "Webhooks by Zapier" app to capture the website URL from the user.
2. The action uses the "Custom Code by Zapier" app to execute Python code that performs the following tasks:
   * Sends a GET request to the sustainability testing API with the website URL as a parameter.
   * Parses the JSON response from the API.
   * Extracts the sustainability score and recommendations.
   * Creates an output message with the scores and recommendations.
   * Returns the output message as the Zap's output data.

### Additional Notes:

* Replace api.example.com with the actual API endpoint for your sustainability testing service.
* This code assumes the API returns a JSON response with the following structure:

```
{
  "score": 85,
  "recommendations": [
    "Reduce image file sizes",
    "Enable browser caching",
    "Optimize CSS and JavaScript files"
  ]
}
```

* You can customize the output message format to your liking.
* You can add additional actions to the Zap to perform further tasks based on the sustainability score and recommendations, such as sending an email notification or creating a task in a project management tool.

**Disclaimer**:

This code is an example and may require modifications depending on the specific API you are using and your desired functionality. Please refer to the documentation for your chosen sustainability testing service.
