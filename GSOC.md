
# AI-Powered API Response to Dynamic UI Generator 

###  Personal Information  
- **Full Name:** Manas M Hejmadi
- **Discord Handle:** `synapse_45006`
- **Github Profile:** https://github.com/synapsecode
- **Resume:** [Link](https://manashejmadi.vercel.app/images/MANAS_RESUME.pdf)
- **Website:** [Link](https://manashejmadi.vercel.app/)
- **Time Zone**: GMT +05:30

### University Information
- **University Name:** Dayananda Sagar College of Engineering (DSCE), Bengaluru
- **Program Enrolled In:** BE in Computer Science & Engineering (CSE)  
- **Year:** Pre-final Year (Third Year)  
- **Expected Graduation Date:** May 2026

### More About Me
I’m Manas M. Hejmadi, a third-year Computer Science & Engineering student at DSCE. With a strong foundation in **Mobile & Desktop Development**, **Backend Engineering**, **Microservices** and **AI Integrations**, I am passionate about building good software at a rapid pace.
I have nearly **6 years of experience in Flutter**, along with extensive experience in other technologies such as **Flask**, **NodeJS**, **NextJS**, **Celery**, **Docker**, **SpringBoot**, **Java**, and **Svelte**. Currently, I lead a team of 3 developers at [Crezam](https://www.crezam.com/), where we are focused on revolutionizing the hiring process through **AI-powered assessments and interviews.**
I also actively participate in hackathons and tech events, earning second place in **IIT Roorkee’s App Innovation Challenge**. I am particularly interested in leveraging AI agents to create more powerful, user-centric applications.

## Project Details  

- **Project Title:** AI-Powered API Response to Dynamic UI Generator   
-  **Issue:** [#617](https://github.com/foss42/apidash/issues/617)
- **Issue Description:**  Develop an AI Agent which transforms API responses into dynamic, user-friendly UI components, enabling developers to visualize and interact with data effortlessly. By analyzing API response structures—such as JSON or XML—the agent automatically generates UI elements like tables, charts, forms, and cards, eliminating the need for manual UI development. One can connect an API endpoint, receive real-time responses, and instantly generate UI components that adapt to the data format. It must also support customization options, allowing developers to configure layouts, styles, and interactive elements such as filters, pagination, and sorting. Finally, users must be able to easily export the generated UI and integrate it in their Flutter or Web apps.

- **Abstract:**  
  This project introduces an AI-powered agent that **automates the transformation of API responses into structured UI schemas and functional UI components** across various frontend frameworks such as ReactJS, Flutter, and HTML. Using Large Language Models (LLMs), the system eliminates the need for manual UI creation by intelligently analyzing API responses and generating corresponding UI structures. Additionally, it enables **dynamic modification of UI components** based on user prompts, allowing for customization in design, layout, and behavior. By seamlessly integrating these capabilities into a unified workflow, the project streamlines frontend development, reducing manual effort and accelerating the UI generation process with a **single-click solution.**

- **Project Goals:**  
  - Conversion from API Response (JSON/XML) into functional UI components such as tables, charts and forms.
  - Provide customization options for layout, styles, and interactive elements.
  - Allow users to **modify UI components through natural language prompts**
  - Generate UI components for multiple frontend frameworks such as Flutter/NextJS etc 
  - **One Click Export** of the generated UI into code
  -   Seamlessly integrate all features into a single, easy-to-use system.
 
## Proposal Description

### Deliverables

 1. **API Response Parser Agent:** Extracts the meaning of the API response (JSON/XML) and converts it into an internal schema representation
 2. **UI Component Generator Agent:** Converts this generated internal schema along with other data like (language/framework etc) into functional UI component code. 
 4. **Component Editor Agent:** This agent accepts UI components generated from the previous step and can make any modification as needed by the user via Natural language prompts
 5. **Gateway Microservice:** This Microservice runs as a Dockerized container and triggers the relevant agent as needed while also taking care of authentication & access management
 6.  **Updated API Dash Client:** The changes needed on the API Dash Client would be modifications to the settings page and creation of a new Dialog with 5-6 new screens (examples provided below)

### Technical Details

#### API Response Parsing
- The API response is first parsed correctly according to its type (JSON / XML) and is then sanitized
    
-   The Agent then goes through the whole response and generates semantic context through inference. This context will be very helpful in determining what is actually needed in the final UI
    
-   The semantic context along with the sanitized response is then sent over to the LLM to be converted into intermediate-representation. This step is essential because it allows us to semantically include/exclude response data leading to good UI generation.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcKv9BOrtB2dCcEZf5bW8mRlQeHgzLK0zX8cuKVMgtWfdBa9AOapFK9-Ay9JbshnRUKTxRQgTz8LODRAv6A5DLTHdwrKP5hXXv-NuSGum6ANhasCe1l5s5s5TBCqWjLnvAyufbueQ?key=IBqKj9gJuYgBFQwMYA4W6Aaw)

#### UI Component Generator Agent
-   Using the internal schema and additional context, the UI Component Generator Agent determines the most appropriate UI component for the data. For example, if the data structure is a List<List>, it can generate a table.
    
-   The agent can further refine this decision based on factors such as data type, layout, and design preferences.
    
-   This component is flexible and can generate code for different frameworks such as NextJS, HTML, or Flutter, with embedded HTTP calls and relevant data-fetching logic.

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd9JEi5Gw1Wb798gNDrM5kUu28kVfDeL4q_dg9gp6o6j7Mg6xz0zyn2ClaSz96ZAs0E3Eaq1DxtxTreNDKZyJCTIe1BJUPD0LNlZ8oXqOwvyBhN12W5vlWpoADFKu_bECvgWxQ4sQ?key=IBqKj9gJuYgBFQwMYA4W6Aaw)**

#### Component Preview & Customization

-   The generated UI component can be previewed in a Component Preview Window, allowing users to inspect the result.
    
-   The Component Editor Agent is an optional feature that allows users to modify the generated component using natural language prompts. This iterative process enables users to refine the UI to meet their exact needs. This is fairly easy to do as most LLMs can already do this. We just have to use extremely good system prompting to get it right
    
-   Common customizations like layout changes, pagination, and sorting can be added through buttons, making the customization process seamless for users.

```dart
componentPreviewOllamaAgent(String component) async {
	 String systemPrompt = 'You are an expert Component Modification….';
	 String semantics = generateSemantics(component);
	 String input = "$systemPrompt\nComponent:$component\n\nSemantics:$semantics";
	 var ollama = Ollama(model: 'QwQ32B-mini');
	 var result = (await ollama.chat(
	   prompt: input,
	 ))['text'];
	 //Check if the result matches our standards
	 String output = governor(result);
	 return output;
}
```

#### Gateway Service (Eventually ported to Dart-Ollama)
-   The Gateway acts as the central API hub, binding all agents into a cohesive system. It will be implemented using Flask or FastAPI, run via Gunicorn in a containerized Docker environment.
    
-   Once the entire architecture has been decided and implemented, this python gateway will be ported to an internal dart-ollama implementation to comply with API Dash’s privacy rules.
    
-   The Gateway will include an encryption-decryption service to ensure that sensitive data in transit is protected using AES-GCM encryption.
    

- A content moderation service will be integrated to prevent malicious or unauthorized prompts from being injected into the system.

#### Privacy and Security
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXenAaJ5JECKHNWzBN_jm6uKKwDmaDgcyxaKmlu_qjqZppZuwBmjXbDrIY2Xp74yVPYH3jCBV0uZaHVNBrhduVPuhRw1wdAiMt584yUEUToaViTykGn0JDaB3K-zCr76Df79BBXskA?key=IBqKj9gJuYgBFQwMYA4W6Aaw)

- The encryption-decryption service uses AES-GCM to encrypt and decrypt payloads, ensuring the confidentiality of data during transmission. The secret keys for encryption are known only to the gateway and the API Dash client, ensuring that user data is protected from external access.
```dart
static  String encrypt(String text) {  
	final key = enc.Key.fromUtf8(ENCKEY);  
	final encrypter = enc.Encrypter(enc.AES(key, mode: enc.AESMode.gcm));  
	final iv = enc.IV.fromUtf8(IVKEY);  
	final encrypted = encrypter.encrypt(text, iv: iv);  
	return encrypted.base64;  
}  
  
static  String? decrypt(String encryptedMessage) {  
	final key = enc.Key.fromUtf8(ENCKEY);  
	final iv = enc.IV.fromUtf8(IVKEY);  
	final encrypter = enc.Encrypter(enc.AES(key, mode: enc.AESMode.gcm));  
	final encryptedBytes = base64.decode(encryptedMessage);  
	final decryptedBytes =  
	encrypter.decryptBytes(enc.Encrypted(encryptedBytes), iv: iv);  
	return utf8.decode(decryptedBytes);  
}
```

## System Architecture

![image info](https://raw.githubusercontent.com/synapsecode/CustomStorage/main/archdiag.png)

## System Workflow and Interactions

![image info](https://raw.githubusercontent.com/synapsecode/CustomStorage/main/sysdiag.png)

### Sample API Response
This is a sample response given when I hit a [DEMO API](https://reqres.in/api/users?page=0)
```json
{
  "page": 1,
  "per_page": 6,
  "total": 12,
  "total_pages": 2,
  "data": [
    {
      "id": 1,
      "email": "george.bluth@reqres.in",
      "first_name": "George",
      "last_name": "Bluth",
      "avatar": "https://reqres.in/img/faces/1-image.jpg"
    },
    {
      "id": 2,
      "email": "janet.weaver@reqres.in",
      "first_name": "Janet",
      "last_name": "Weaver",
      "avatar": "https://reqres.in/img/faces/2-image.jpg"
    },
   ...
  ]
}
```
This is subsequently sent to our frontend prompt where we accept the target framework, let's assume the user has selected `Flutter` 
This response will first be sent into our Semantic Agent which tries to pick out what this response means.

### Generated Semantic Response
<code>
The response provides paginated user data with information such as the current page (1), number of users per page (6), total users (12), and total pages (2). Each user entry includes an ID, email, first name, last name, and an avatar image URL. For the UI, display the user's name (first and last), email, and avatar, with avatars shown as circular images. Pagination controls should be implemented with "Previous" and "Next" buttons, as well as page numbers based on the current page and total pages. Ensure the UI dynamically adjusts to show 6 users per page and maintain visual clarity with proper spacing for readability. The design should effectively present the user data while allowing easy navigation between pages.
</code>

### Generated Internal Representation
```json
[{
    "type": "column",
    "elements": [
      {
        "type": "row",
        "elements": [
          {
            "type": "image",
            "src": "https://reqres.in/img/faces/7-image.jpg",
            "shape": "circle",
            "width": "60",
            "height": "60"
          },
          {
            "type": "column",
            "elements": [
              {
                "type": "text",
                "data": "Michael Lawson",
                "font": "segoe-ui",
                "color": "blue"
              },
              {
                "type": "text",
                "data": "michael.lawson@reqres.in",
                "font": "segoe-ui",
                "color": "gray"
              }
            ]
          }
        ]
      },
      {
        "type": "vspacer",
        "spacing": "20"
      },
      ...
    ]
  },
  {
    "type": "vspacer",
    "spacing": "30"
  },
  {
    "type": "row",
    "elements": [
      {
        "type": "button",
        "text": "Previous",
        "onclick": "page=1"
      },
     ...
   ]
  },
  ...
  {
    "type": "link",
    "text": "Tired of writing endless social media content? Let Content Caddy generate it for you.",
    "url": "https://contentcaddy.io?utm_source=reqres&utm_medium=json&utm_campaign=referral"
}]
```
### Generated Flutter Component
```dart
class UserListWidget extends StatelessWidget {
  final List<Map<String, dynamic>> users = [
    {
      'firstName': 'Michael',
      'lastName': 'Lawson',
      'email': 'michael.lawson@reqres.in',
      'avatar': 'https://reqres.in/img/faces/7-image.jpg',
    },
    // Add more users here...
  ];

  final int currentPage;
  final int totalPages;
  final Function(int) onPageChange;

  UserListWidget({
    required this.currentPage, 
    required this.totalPages, 
    required this.onPageChange,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Column(
          children: users.map((user) {
            return Row(
              children: [
                CircleAvatar(
                  radius: 30, 
                  backgroundImage: NetworkImage(user['avatar']),
                ),
                SizedBox(width: 16),
                Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      '${user['firstName']} ${user['lastName']}',
                      style: TextStyle(fontFamily: 'segoe-ui', color: Colors.blue),
                    ),
                    Text(
                      user['email'],
                      style: TextStyle(fontFamily: 'segoe-ui', color: Colors.grey),
                    ),
                  ],
                ),
              ],
            );
          }).toList(),
        ),
        SizedBox(height: 10),
        Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: currentPage > 1 ? () => onPageChange(currentPage - 1) : null,
              child: Text('Previous'),
            ),
            SizedBox(width: 8),
            ElevatedButton(
              onPressed: () => onPageChange(1),
              child: Text('1'),
            ),
            SizedBox(width: 8),
            ElevatedButton(
              onPressed: () => onPageChange(2),
              child: Text('2'),
            ),
            SizedBox(width: 8),
            ElevatedButton(
              onPressed: currentPage < totalPages ? () => onPageChange(currentPage + 1) : null,
              child: Text('Next'),
            ),
          ],
        ),
        SizedBox(height: 10),
        TextButton(
          onPressed: () {
            // Open the link
          },
          child: Text(
            'Tired of writing endless social media content? Let Content Caddy generate it for you.',
            style: TextStyle(color: Colors.blue),
          ),
        ),
      ],
    );
  }
}
```

### Sample Output (Flutter)
![image info](https://raw.githubusercontent.com/synapsecode/CustomStorage/main/flutex.jpg)

Now that the output is in Flutter Code, It can easily be into the next bot **Component Editor Agent**) which can easily make any changes that you request via *Natural Language*

## Sample UI 

![image info](https://raw.githubusercontent.com/synapsecode/CustomStorage/main/framwdiag.png)
![image info](https://raw.githubusercontent.com/synapsecode/CustomStorage/main/LDG.png)
![image info](https://raw.githubusercontent.com/synapsecode/CustomStorage/main/compo.png)
If any modifications are made, it will go back to loading screen and come back to this page with the relevant modifications.
When Export is clicked, the relevant generated code is **copied to clipboard** and the dialog closes!

## Week-wise Breakdown

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeW9foRSNKENCLkJ_Mx69P2JRG-PY-HcSCiRYGsVeGOwUyxZoIhcPu08sUmzAnUKP-nXDi8-0g_lBHj4lbZLklSAB1CD3SaRpHYWYVoVT7f8V_fu4pAgzFqLK2xVv-js8i5wOMI4Q?key=IBqKj9gJuYgBFQwMYA4W6Aaw)

### Week 1 (Community Bonding)

 - Get in touch with the developers and the mentor
   
  -   Introduction to the community, to the mentor and fix timings to communicate
   
  -   Set communication schedules for regular updates.
   
   -   Go through the API Dash codebase and dependency packages, especially the structure around melos.
   
   -   Complete local setup (macOS platform support) and run the product in DEBUG mode.
   
   -   Discuss any suggestions and changes to the project. There could modifications, new additions or amendments; it would be better to go
   over these early
   
   -   Understand the state management approaches and dependency structure.
   
   -   Discuss the working of the existing application and the implementation of the new features with the mentor

    

###  Week 2 (API Response Parser Agent Development)

-   Discuss the existing application with the mentor and plan the development of the API Response Parser Agent.
    
-   Understand how to map API response formats (JSON/XML) into internal schema representation.
    
-   Implement code to extract meaningful data from JSON responses.
    
-   Research on which API response data meaningfully contributes to the final UI and which data is irrelevant. This can help in improving user satisfaction.
    
-   Start testing with real-world API responses to ensure proper parsing and extraction of meaningful data.
    

### Week 3 (UI Component Generator Agent Development)

-   Design a system to convert the internal schema into UI components such as tables and charts
    
-   Implement a basic conversion for a sample API response to UI components (start with JSON → Table).
    
-   Build the UI Component Generator Agent that generates frontend code from the internal schema.
    
-   Develop a testing plan to ensure the generated UI components match the expected functionality
    
-   Test and validate generated components in a sample frontend
    

### Week 4 (Customization & Natural Language Interaction)

-   Implement options to modify styles such as color, size, and borders for UI components.
    
-   Develop the Component Editor Agent, which allows the user to modify UI components via natural language prompts.
    
-   Integrate Generative AI to allow users to modify the generated UI components, such as resizing, renaming, or changing themes using plain text
    
-   Develop a testing strategy for different types of modifications users may request.
    
-   Discuss any breaking changes that would arise in the UI because of these integrations
    

  

### Week 5 (Multi-framework Support & Export Mechanism)

-   Extend the UI component generation to support React, Flutter (or other frameworks based on project needs).
    
-   Extend the Response Parser to support XML API Responses
    
-   Handle different XML Schema variations, ensuring flexibility in data extraction
    
-   Implement error handling mechanisms for poorly formatted XML
    
-   Implement a One Click Export button that generates the final UI code in the selected framework.
    
-   Test the generated export functionality by testing it in other framework specific codebases
    

### Week 6 (Conversion to dart-Ollama & local inference)

-   Work on making the AI service locally-runnable using dart-ollama, ensuring that API dash remains committed to its privacy-first goal.
    
-   Allow direct AI Model selection in combination with custom LLM API keys to be set up from the settings page
    
-   Implement privacy features so the LLM keys can be managed securely by users.
    

  

### Week 7 (User interface Updates & Testing)

-   Modify the **API Dash Client**, including changes to the settings page and a new dialog for UI Generation.
    
-   Migrate from the testing mockup screens to actual full fledged designs pre-approved by the mentor
    
-   Conduct tests of the entire flow: from API response parsing, schema generation, UI component generation, customization, and exporting.
    
-   Gather feedback from the mentor and iterate on any issues or improvements.
    

### Week 8 (Final Buffer, Optimization & Report)

-   Use this buffer for any remaining tasks, bug fixing, or addressing missed items.
    
-   Document all the components built, their functionality, and how they can be used.
    
-   Discuss the project progress with the mentor and provide a plan for any future enhancements.
    
-   Write the final project report covering key achievements, challenges faced, and any future plans.
    

  

## 7. Relevant Skills and Experience

  
### 7.1. Skills

- Languages: Dart, Python, Java, Javascript, Typescript, Go

- Frameworks / Libraries: Flutter, Flask, FastAPI, SpringBoot, ReactJS, NextJS, Tailwind

- Databases & Tools: PostgreSQL, Supabase, Firebase, MongoDB, Git, Docker, Cloudflare Workers, Dify.ai, n8n

  

### 7.2 Experience

-   Lead Developer at [Crezam](https://crezam.com) (AI Powered assessments and interview platform); Extensive work on Flutter for Mobile, Web and MacOS platforms
    
-   Flutter Developer for [Atlas Knowledge](https://atlas.fm/) which is a bookmark based second-brain
    
-   Flutter Developer at [FitchoiceWorld](https://www.fitchoiceworld.com/) which is a fitness centre booking & wellness application
    
-   I have a lot of experience with Flutter Web and MacOS including creation of platform specific code and then calling them via bridges/interop channels.
    

  

## 8. Projects

- **Microblogger**
	- A full scale social media application built using Flutter and Flask which includes a wide variety of posts, stories and interactive content fully built from scratch. The whole process was thoroughly documented under my 100 days of code challenge.

	- Github: [https://github.com/synapsecode/MicrobloggerV1](https://github.com/synapsecode/MicrobloggerV1)

	- Public Build Log: [https://tulip-quality-7a5.notion.site/Microblogger-Builds-0b6f44eece5b419ca57f8a431e03ad2c](https://tulip-quality-7a5.notion.site/Microblogger-Builds-0b6f44eece5b419ca57f8a431e03ad2c)

  

- **FFPNS (Flutter Firebase PushNotifications Service)**

	- A package that I created to make it extremely simple to integrate firebase push notifications into a fluter app with minimal setup & advanced features such as background message handling and so on

	- Github: [https://github.com/synapsecode/ffpns](https://github.com/synapsecode/ffpns)
  


### Motivation & Past Experience

1. Have you worked on or contributed to a FOSS project before? Can you attach repo links or relevant PRs? 
	- **ANS**: Have worked on large codebases but no direct experience with contribution.
2. What is your one project/achievement that you are most proud of? Why?
	- **ANS**: I am most proud of my microblogging application, _Microblogger_. It was a large project I undertook with the goal of learning Flutter, and through it, I gained a deep understanding of the framework. What makes me most proud is the Instagram-like story editor that I built within the app. It was a challenging feature that required creative problem-solving, and successfully implementing it taught me a lot.
3. What kind of problems or challenges motivate you the most to solve them?
	- **ANS**:  I am particularly motivated by challenges that address real pain points for users, as solving these issues has a direct, meaningful impact. I thrive on problems that require creative and efficient solutions, especially when they need to scale effectively. Additionally, I enjoy opportunities that allow me to take ownership of the project and make critical decisions that shape the direction of the solution.
4. Will you be working on GSoC full-time? In case not, what will you be studying or working on while working on the project?
	- **ANS**: Yes, I will primarily be working full-time on GSoC. Occasionally, I may have exams, course projects, or job/internship responsibilities, but they will not impact my commitment to GSoC.
5. Do you mind regularly syncing up with the project mentors?
	- **ANS**:  I am available after 5PM IST and can get on calls when needed
6. What interests you the most about API Dash?
	- **ANS**: It has a clean and minimalist design with a super fast startup time. It does exactly what it's meant to and gets straight to the point! Plus, Flutter and Dart hold a special place for me since they helped me land my first gig. I've really been enjoying using API Dash.
7. Can you mention some areas where the project can be improved?
	- **ANS**: One feature that I wanted to include is code to collection generation. Where, we just put the flask code (for example) into the textbox and the LLM agent automatically creates relevant Requests and Collections. This would speed up development and testing time.

