automation-qa-assessment/
│
├── README.md
│
├── task1/
│   ├── Task1_QA_Report_Karishma_Singhi.pdf
│   └── screenshots/
│
├── task2/
│   ├── Task2_Workflow_Karishma.json
│   ├── workflow-canvas.png
│   └── successful-execution.png
│
├── bonus/
│   ├── Bonus_UptimeMonitor_Karishma.json
│   └── uptime-monitor.png
│
└── docs/
    └── architecture-diagram.png


Hello, my name is Karishma Singhi.

This submission contains both the QA assessment and the n8n automation workflow.

First, for Task 1, I tested the RealWorld demo application.

I covered the major user journeys including registration, login, article creation, editing, deletion, and logout.

During testing, I identified several issues related to session management, validation, accessibility, and user experience.

One issue I analyzed in detail was session expiration handling. The application does not provide a clear user experience when authentication tokens expire, which can lead users to believe the application is broken.

Moving to Task 2.

I built an n8n workflow that runs every hour.

The workflow retrieves trending cryptocurrencies from CoinGecko.

The results are filtered to keep only the top five coins.

For each coin, a second API request retrieves detailed market information.

An IF node checks whether the 24-hour price change exceeds a configured threshold.

If the condition is met, a digest message is generated and sent to Discord.

To improve reliability, all API calls use error handling and failed requests are routed to a dedicated error workflow.

For Google Sheets logging, failed executions are recorded with timestamps, workflow names, and error messages.

For the bonus task, I created an uptime monitor workflow.

The monitor runs every five minutes and sends an HTTP request to the target application.

If the response status is not 200, a Discord alert is triggered.

The workflow also captures response times and can be extended to generate daily availability reports.

Thank you for reviewing my submission.

Google Sheets Logging Workflow

Add these nodes:

Error Trigger
Set Node
{
  "timestamp": "{{$now}}",
  "workflow": "{{$workflow.name}}",
  "error": "{{$json.error.message}}"
}
Google Sheets Node

Operation:

Append Row

Columns:

Timestamp	Workflow	Error
{{$now}}	{{$workflow.name}}	{{$json.error.message}}
Separate Error Trigger Workflow
Error Trigger
      ↓
Set Error Details
      ↓
Google Sheets Log
      ↓
Discord Alert

Discord Message:

🚨 Workflow Failure

Workflow: {{$workflow.name}}

Error: {{$json.error.message}}

Time: {{$now}}
Bonus Uptime Monitor Workflow
Schedule Trigger (Every 5 Minutes)
            ↓
HTTP Request
            ↓
IF Status Code != 200
      ↙              ↘
 Alert Path       Success Path
      ↓                ↓
Discord Alert     Store Metrics
HTTP Request

URL:

https://demo.realworld.io
IF Condition
{{$json.statusCode}} != 200
Alert Message
🚨 Website Down Alert

Site: RealWorld Demo

Status: {{$json.statusCode}}

Response Time: {{$json.responseTime}} ms

Time: {{$now}}
