# Context for case study

## General

Covestro is a chemical company operating in Europe (Germany), the US, Asia and China. Although the official company language is English, employees in Europe, e.g. Germany, Spain and Asia prefer the local languages for communication. The AI department has already built an internal AI assistant called CoVA (Covestro Virtual Assistant). Currently mostly employees in offices use the assistant via text input. The challenge is to build a voice mode (aking to [ChatGPT voice mode][1]) so that users in constraint environments (labs, plants, etc.) and users unable to type (because they are driving or wearing protective equipment) can also use the assistant to get relevant information when they need it.

## Thoughts

- The main challenge is to make voice mode work reliably in the target environments of a chemical company. Plants are noisy, internet and intranet connection is often limited (in terms of bandwidth and availability), employees often must use protective equipment, there are safety rules which forbid or strictly limit the usage of electronic device such as mobile phones. If devices are permitted, the often need to be [EX certified][2].
- Even if mobile phones can be used, employees wearing protective equipment (gloves, glasses, ear plugs, helmet, etc.) may have a hard time pressing a button to activate/deactivate voice mode. Eventually voice mode could be activated via geo location. It may be interesting to use certified in ear headphones for communication with the agent (still requires some kind of mobile computer)

## Assumptions

- An internal assistant (CoVA) already exists and can be used as the backend. CoVA covers features such as authentication and authorization, handling of text input and AI model responses, message history, access to internal knowledge and internal systems. The real challenge is to reliable build voice-based interaction with it.
- The agent will fist and foremost support English as the main language for interaction
- Data privacy is of limited importance for this issue. Employees eager to use the voice AI service are eligible to do so.
- Covestro does hand out mobile phones (Android/iOS) to employees which are managed by mobile device management (MDM)

## Tech stack

Covestro is uses AWS extensively and only relies on Azure for Entra ID. CoVA consists of a Angular frontend written in TypeScript and a multiple AWS services (Lambda, S3, IAM, Secretsmanager, Parameter Store, Bedrock, etc.) The backend code is also written in TypeScript, as is the IaC code, where AWS CDK is used. Source code is managed in GitHub, CI uses GitHub Actions in combination with AWS CDK for deployment. Covestro used AWS Bedrock to consume AI models like Claude.

AWS recently released [Nova Sonic models][3] which can be used to implement voice mode. Refer to the [model card][4], [user guide][5] and [code examples][6] for further information. An alternative could be using on device models, such as models from NVIDIA (Parakeet), to convert speech to text and only send text to AWS. This could be useful to handle connectivity issues (text is smaller in size than audio) and reduce costs (AWS charges for data input)


## Constraints

## Research


[1]: https://chatgpt.com/features/voice/
[2]: https://www.iecex.com/
[3]: https://aws.amazon.com/blogs/aws/introducing-amazon-nova-sonic-human-like-voice-conversations-for-generative-ai-applications/
[4]: https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-amazon-nova-sonic.html
[5]: https://docs.aws.amazon.com/nova/latest/userguide/speech-code-examples.html
[6]: https://github.com/aws-samples/amazon-nova-samples/tree/main/speech-to-speech/sample-codes/websocket-nodejs/sr
