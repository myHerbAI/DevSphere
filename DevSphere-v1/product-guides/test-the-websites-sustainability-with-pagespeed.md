---
cover: ../.gitbook/assets/_107.png
coverY: -25
---

# Test the website's sustainability with PageSpeed

```python
import requests
from zapier.platform.app import Zap
from zapier.platform.intent import Intent
from zapier.platform.user import User

class GooglePageSpeedInsights(Zap):
    """
    Zapier integration for Google PageSpeed Insights
    """

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.api_key = self.config.get('api_key')
        if not self.api_key:
            raise ValueError('Please provide a Google PageSpeed Insights API key in the Zapier app settings.')

    def create(self, data):
        """
        Trigger to get Google PageSpeed Insights data
        """
        url = data['url']
        api_url = f"https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url={url}&key={self.api_key}"
        response = requests.get(api_url)
        response.raise_for_status()
        data = response.json()
        return {
            'url': url,
            'desktop_score': data['lighthouseResult']['categories']['performance']['score'],
            'mobile_score': data['lighthouseResult']['categories']['performance']['score'],
            'desktop_recommendations': data['lighthouseResult']['audits'],
            'mobile_recommendations': data['lighthouseResult']['audits'],
        }

    def trigger(self):
        """
        Defines the trigger for this Zap
        """
        return Intent(
            name='Get PageSpeed Insights Data',
            input_fields=[
                {
                    'key': 'url',
                    'label': 'Website URL',
                    'required': True,
                    'help_text': 'Enter the URL of the website to analyze.',
                },
            ],
            perform=self.create,
        )

    def resource(self, **kwargs):
        """
        Defines the resource for this Zap
        """
        return {
            'desktop_score': self.get_score('desktop'),
            'mobile_score': self.get_score('mobile'),
            'desktop_recommendations': self.get_recommendations('desktop'),
            'mobile_recommendations': self.get_recommendations('mobile'),
        }

    def get_score(self, device):
        """
        Helper function to retrieve the PageSpeed score for the specified device
        """
        return self.data[f'{device}_score']

    def get_recommendations(self, device):
        """
        Helper function to retrieve the PageSpeed recommendations for the specified device
        """
        return [
            {
                'title': audit['title'],
                'description': audit['description'],
            }
            for audit in self.data[f'{device}_recommendations']
        ]

# This is the main entry point for the Zapier integration
if __name__ == '__main__':
    GooglePageSpeedInsights.run()
```

**Explanation:**

1. **Import necessary libraries:**
   * `requests` for making HTTP requests to the Google PageSpeed Insights API
   * `zapier.platform.app` for defining the Zapier application
   * `zapier.platform.intent` for defining the trigger intent
   * `zapier.platform.user` for handling user authentication (not used in this example)
2. **Define the `GooglePageSpeedInsights` class:**
   * Inherits from the `Zap` class to create the Zapier integration.
   * Initializes with the API key from the Zapier app settings.
   * Raises an error if no API key is provided.
3. **`create()` function:**
   * Defines the trigger logic for the Zap.
   * Takes the website URL as input.
   * Makes an HTTP GET request to the Google PageSpeed Insights API using the provided URL and API key.
   * Processes the API response to extract the desktop and mobile scores and recommendations.
   * Returns the extracted data to the Zap.
4. **`trigger()` function:**
   * Defines the trigger intent for the Zap.
   * Sets the name of the trigger to "Get PageSpeed Insights Data".
   * Defines an input field for the website URL.
   * Sets the `perform` function to the `create()` function.
5. **`resource()` function:**
   * Defines the resource for the Zap.
   * Returns the desktop and mobile scores and recommendations.
   * Uses helper functions `get_score()` and `get_recommendations()` to retrieve the data from the `data` attribute.
6. **`get_score()` and `get_recommendations()` helper functions:**
   * Extract the scores and recommendations from the `data` attribute based on the specified device (desktop or mobile).
7. **`if __name__ == '__main__':` block:**
   * This block runs the Zapier integration when the script is executed.

**To install this integration in Zapier:**

1. Create a new Zapier app.
2. Add a new trigger and select "Google PageSpeed Insights" from the list of apps.
3. Configure the app with your Google PageSpeed Insights API key.
4. Choose the "Get PageSpeed Insights Data" trigger.
5. Configure the trigger with the website URL you want to analyze.
6. Add an action to your Zap and select the "Google PageSpeed Insights" app again.
7. Choose the desired action, such as sending the PageSpeed scores to a Slack channel or Google Sheet.
8. Configure the action and connect your Zap to the chosen service.
9. Test your Zap to ensure it works correctly.

**Note:** Before using this integration, you must obtain a Google PageSpeed Insights API key from the Google Cloud Platform console.
