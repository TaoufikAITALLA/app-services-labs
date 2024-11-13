# Running JMeter Test Plan on Azure Load Testing

To run this JMeter test plan on Azure Load Testing, you need to ensure that the environment variables referenced in the test plan are properly set up in Azure. Here are the steps to configure Azure Load Testing for your JMeter test plan:

## Create an Azure Load Testing Resource

If you haven't already, create an Azure Load Testing resource in the Azure portal.

## Upload the JMeter Test Plan

1. Navigate to the Azure Load Testing resource.
2. Go to the "Tests" section and create a new test.
3. Upload your `SampleApp.jmx` file.

## Set Environment Variables

In the test configuration, you need to set the environment variables that your JMeter test plan uses. These variables include `threads_per_engine`, `ramp_up_time`, `duration_in_sec`, `domain`, `protocol`, and `path`.

You can set these environment variables in the "Parameters" section of the test configuration. For example:

- `threads_per_engine`: 10
- `ramp_up_time`: 60
- `duration_in_sec`: 300
- `domain`: your-web-app.azurewebsites.net
- `protocol`: https
- `path`: /

## Configure Test Settings

Specify the number of engine instances and other test parameters as needed. This will determine the scale of your load test.

## Run the Test

Start the load test from the Azure portal. Monitor the test execution and view the results in real-time.

## Analyze Results

After the test completes, analyze the results provided by Azure Load Testing to understand the performance and behavior of your Azure Web App under load.

By ensuring that the environment variables are correctly set in the Azure Load Testing configuration, you can effectively run your JMeter test plan without needing to change the test plan itself.