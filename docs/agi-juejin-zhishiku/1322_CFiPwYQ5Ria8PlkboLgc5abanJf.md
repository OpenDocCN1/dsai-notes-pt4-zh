# 14. Claude官方提示词

> 来源：[https://agijuejin.feishu.cn/docx/TALydkQxPoVEiTx2I5YcgkxNnjb](https://agijuejin.feishu.cn/docx/TALydkQxPoVEiTx2I5YcgkxNnjb)

# Cosmic keystrokes

Generate an interactive speed typing game in a single HTML file, featuring side-scrolling gameplay and Tailwind CSS styling.

prompt：

> Write me a fully complete web app as a single HTML file. The app should contain a simple side-scrolling game where I use WASD to move around. When moving around the world, occasionally the character/sprite will encounter words. When a word is encountered, the player must correctly type the word as fast as possible.The faster the word is successfully typed, the more point the player gets. We should have a counter in the top-right to keep track of points. Words should be random and highly variable to keep the game interesting.

>

> You should make the website very aesthetic and use Tailwind.

Example output:

```
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset=&quot;UTF-8&quot;&gt;
  &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
  &lt;script src=&quot;https://cdn.tailwindcss.com&quot;&gt;&lt;/script&gt;
  &lt;title&gt;Side-Scrolling Typing Game&lt;/title&gt;
&lt;/head&gt;
&lt;body class=&quot;bg-gray-900 text-white&quot;&gt;
  &lt;div class=&quot;fixed top-4 right-4 text-2xl&quot;&gt;Score: &lt;span id=&quot;score&quot;&gt;0&lt;/span&gt;&lt;/div&gt;
  &lt;div id=&quot;game&quot; class=&quot;h-screen w-screen overflow-hidden relative&quot;&gt;
    &lt;div id=&quot;player&quot; class=&quot;absolute bottom-10 left-10 w-8 h-8 bg-blue-500&quot;&gt;&lt;/div&gt;
  &lt;/div&gt;
  &lt;div id=&quot;word-input&quot; class=&quot;fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 bg-white text-black p-4 rounded hidden&quot;&gt;
    &lt;input type=&quot;text&quot; id=&quot;typed-word&quot; class=&quot;border border-gray-300 rounded px-2 py-1 mb-2 w-full&quot;&gt;
    &lt;button id=&quot;submit-word&quot; class=&quot;bg-blue-500 text-white px-4 py-2 rounded&quot;&gt;Submit&lt;/button&gt;
  &lt;/div&gt;
  &lt;script&gt;
    const words = [&#x27;ability&#x27;, &#x27;able&#x27;, &#x27;about&#x27;, &#x27;above&#x27;, &#x27;accept&#x27;, &#x27;according&#x27;, &#x27;account&#x27;, &#x27;across&#x27;, &#x27;action&#x27;, &#x27;activity&#x27;, &#x27;actually&#x27;, &#x27;address&#x27;, &#x27;administration&#x27;, &#x27;admit&#x27;, &#x27;adult&#x27;, &#x27;affect&#x27;, &#x27;after&#x27;, &#x27;again&#x27;, &#x27;against&#x27;, &#x27;agency&#x27;, &#x27;agent&#x27;, &#x27;ago&#x27;, &#x27;agree&#x27;, &#x27;agreement&#x27;, &#x27;ahead&#x27;, &#x27;air&#x27;, &#x27;all&#x27;, &#x27;allow&#x27;, &#x27;almost&#x27;, &#x27;alone&#x27;, &#x27;along&#x27;, &#x27;already&#x27;, &#x27;also&#x27;, &#x27;although&#x27;, &#x27;always&#x27;, &#x27;American&#x27;, &#x27;among&#x27;, &#x27;amount&#x27;, &#x27;analysis&#x27;, &#x27;and&#x27;, &#x27;animal&#x27;, &#x27;another&#x27;, &#x27;answer&#x27;, &#x27;any&#x27;, &#x27;anyone&#x27;, &#x27;anything&#x27;, &#x27;appear&#x27;, &#x27;apply&#x27;, &#x27;approach&#x27;, &#x27;area&#x27;, &#x27;argue&#x27;, &#x27;arm&#x27;, &#x27;around&#x27;, &#x27;arrive&#x27;, &#x27;art&#x27;, &#x27;article&#x27;, &#x27;artist&#x27;, &#x27;as&#x27;, &#x27;ask&#x27;, &#x27;assume&#x27;, &#x27;at&#x27;, &#x27;attack&#x27;, &#x27;attention&#x27;, &#x27;attorney&#x27;, &#x27;audience&#x27;, &#x27;author&#x27;, &#x27;authority&#x27;, &#x27;available&#x27;, &#x27;avoid&#x27;, &#x27;away&#x27;, &#x27;baby&#x27;, &#x27;back&#x27;, &#x27;bad&#x27;, &#x27;bag&#x27;, &#x27;ball&#x27;, &#x27;bank&#x27;, &#x27;bar&#x27;, &#x27;base&#x27;, &#x27;be&#x27;, &#x27;beat&#x27;, &#x27;beautiful&#x27;, &#x27;because&#x27;, &#x27;become&#x27;, &#x27;bed&#x27;, &#x27;before&#x27;, &#x27;begin&#x27;, &#x27;behavior&#x27;, &#x27;behind&#x27;, &#x27;believe&#x27;, &#x27;benefit&#x27;, &#x27;best&#x27;, &#x27;better&#x27;, &#x27;between&#x27;, &#x27;beyond&#x27;, &#x27;big&#x27;, &#x27;bill&#x27;, &#x27;billion&#x27;, &#x27;bit&#x27;, &#x27;black&#x27;, &#x27;blood&#x27;, &#x27;blue&#x27;, &#x27;board&#x27;, &#x27;body&#x27;, &#x27;book&#x27;, &#x27;born&#x27;, &#x27;both&#x27;, &#x27;box&#x27;, &#x27;boy&#x27;, &#x27;break&#x27;, &#x27;bring&#x27;, &#x27;brother&#x27;, &#x27;budget&#x27;, &#x27;build&#x27;, &#x27;building&#x27;, &#x27;business&#x27;, &#x27;but&#x27;, &#x27;buy&#x27;, &#x27;by&#x27;, &#x27;call&#x27;, &#x27;camera&#x27;, &#x27;campaign&#x27;];

    let score = 0;
    let currentWord;
    let startTime;

    const game = document.getElementById(&#x27;game&#x27;);
    const player = document.getElementById(&#x27;player&#x27;);
    const wordInput = document.getElementById(&#x27;word-input&#x27;);
    const typedWord = document.getElementById(&#x27;typed-word&#x27;);
    const submitWord = document.getElementById(&#x27;submit-word&#x27;);
    const scoreDisplay = document.getElementById(&#x27;score&#x27;);

    let playerX = 10;
    let playerY = game.offsetHeight - 50;
    const speed = 5;

    function placeWord() {
      const word = document.createElement(&#x27;div&#x27;);
      word.textContent = words[Math.floor(Math.random() * words.length)];
      word.className = &#x27;absolute text-2xl&#x27;;
      word.style.left = `${game.offsetWidth + 10}px`;
      word.style.top = `${Math.random() * (game.offsetHeight - 50)}px`;
      game.appendChild(word);
      return word;
    }

    function checkCollision(word) {
      const playerRect = player.getBoundingClientRect();
      const wordRect = word.getBoundingClientRect();
      return !(
        playerRect.right &lt; wordRect.left ||
        playerRect.left &gt; wordRect.right ||
        playerRect.bottom &lt; wordRect.top ||
        playerRect.top &gt; wordRect.bottom
      );
    }

    function startInput(word) {
      currentWord = word.textContent;
      wordInput.style.display = &#x27;block&#x27;;
      typedWord.value = &#x27;&#x27;;
      typedWord.focus();
      startTime = Date.now();
    }

    function endInput() {
      wordInput.style.display = &#x27;none&#x27;;
      const endTime = Date.now();
      const elapsedTime = endTime - startTime;
      const timeBonus = Math.max(2000 - elapsedTime, 0);
      score += Math.round(1000 + timeBonus);
      scoreDisplay.textContent = score;
    }

    function checkInput() {
      if (typedWord.value === currentWord) {
        endInput();
      }
    }

    function gameLoop() {
      playerY = Math.max(0, Math.min(playerY, game.offsetHeight - player.offsetHeight));
      playerX = Math.max(0, Math.min(playerX, game.offsetWidth - player.offsetWidth));
      player.style.top = `${playerY}px`;
      player.style.left = `${playerX}px`;

      const words = Array.from(document.querySelectorAll(&#x27;#game &gt; div:not(#player)&#x27;));
      words.forEach(word =&gt; {
        const currentLeft = parseInt(word.style.left);
        word.style.left = `${currentLeft - speed}px`;
        if (checkCollision(word)) {
          startInput(word);
        }
        if (currentLeft &lt; -word.offsetWidth) {
          word.remove();
        }
      });

      if (Math.random() &lt; 0.01) {
        placeWord();
      }

      requestAnimationFrame(gameLoop);
    }

    document.addEventListener(&#x27;keydown&#x27;, e =&gt; {
      if (e.key === &#x27;w&#x27;) playerY -= speed;
      if (e.key === &#x27;a&#x27;) playerX -= speed;
      if (e.key === &#x27;s&#x27;) playerY += speed;
      if (e.key === &#x27;d&#x27;) playerX += speed;
    });

    typedWord.addEventListener(&#x27;input&#x27;, checkInput);
    submitWord.addEventListener(&#x27;click&#x27;, checkInput);

    gameLoop();
  &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

API request

Python:

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=2000,
    temperature=0,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Write me a fully complete web app as a single HTML file. The app should contain a simple side-scrolling game where I use WASD to move around.  When moving around the world, occasionally the character/sprite will encounter words. When a word is encountered, the player must correctly type the word as fast as possible.The faster the word is successfully typed, the more point the player gets. We should have a counter in the top-right to keep track of points. Words should be random and highly variable to keep the game interesting. \n\nYou should make the website very aesthetic and use Tailwind.&quot;}
    ]
)
print(message.content)
```

TypeScript:

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 2000,
  temperature: 0,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Write me a fully complete web app as a single HTML file. The app should contain a simple side-scrolling game where I use WASD to move around.  When moving around the world, occasionally the character/sprite will encounter words. When a word is encountered, the player must correctly type the word as fast as possible.The faster the word is successfully typed, the more point the player gets. We should have a counter in the top-right to keep track of points. Words should be random and highly variable to keep the game interesting. \n\nYou should make the website very aesthetic and use Tailwind.&quot;}
  ]
});
console.log(msg);
```

# Corporate clairvoyant

Extract insights, identify risks, and distill key information from long corporate reports into a single memo.

prompt：

> Your task is to analyze the following report:

> <report>

> [Full text of Matterport SEC filing 10-K 2023, not pasted here for brevity]

> </report>

> 

> Summarize this annual report in a concise and clear manner, and identify key market trends and takeaways. Output your findings as a short memo I can send to my team. The goal of the memo is to ensure my team stays up to date on how financial institutions are faring and qualitatively forecast and identify whether there are any operating and revenue risks to be expected in the coming quarter. Make sure to include all relevant details in your summary and analysis.

Example output：

> To: Investment Team

> From: [Your Name]

> Date: March 4, 2024

> Subject: Matterport 2022 10-K Highlights and Financial Outlook

> Hi team,

> I have identified some key takeaways from Matterport's 2022 annual report:

> Financial Performance:

+   Revenue increased 22% to $136.1M in 2022, driven by growth in subscription and services revenue

+   However, gross margin decreased from 54% to 38% due to increased costs from product revenue and supply chain challenges

+   Operating losses widened to $275.5M as the company invested heavily in R&D and sales & marketing to drive growth

+   Ended 2022 with $476.9M in cash and investments to fund operations

> Business Highlights:

+   Subscriber base grew 39% to over 701,000 at end of 2022. Spaces under management reached 9.2 million

+   Expanded platform with launch of Matterport Axis motorized mount, Pro3 camera, and major updates to cloud software

+   Completed acquisitions of Enview (AI for 3D spatial data) and VHT Studios (real estate marketing) to enhance offerings

+   Signed notable partnerships with Burns & McDonnell and CGS to deliver Matterport's solutions to their enterprise customers

> Outlook:

+   Matterport sees a massive market opportunity in digitizing the world's buildings and spaces (estimates $240B+ TAM)

+   Focused on driving adoption with large enterprise customers across key verticals like real estate, AEC, facilities management, and more

+   Will continue investing in R&D and international expansion to penetrate the largely untapped global building market

+   Near-term growth and margins may be impacted by macroeconomic headwinds and supply chain issues

+   However, healthy liquidity position and relatively sticky customer base provide downside protection

> In summary, Matterport delivered strong top-line growth in 2022 as it made progress penetrating a huge global market. While profitability remains challenged as the company invests heavily for growth, its balance sheet looks solid. Apart from some ongoing supply chain issues, the underlying business momentum appears intact based on key operating metrics. Barring a severe economic downturn, Matterport seems well-positioned to continue gaining share in the nascent building digitization space.

API request

Python：

```

```

TypeScrip

```

```

# Website wizard

Create one-page websites based on user specifications.

prompt：

1.  Create a one-page website for an online learning platform called "EduQuest" with the following features and sections:

A fixed navigation bar with links to course categories (Math, Science, Languages, Arts) and a search bar.

A hero section with a video background showcasing students learning online, a dynamic tagline that rotates between "Learn at your own pace," "Discover new passions," and "Expand your horizons" every 3 seconds, and a "Get Started" button leading to a course catalog.

A featured courses section displaying course cards with placeholders for course images, titles, instructors, and descriptions.

An interactive "Learning Paths" section with a short quiz to determine learning styles and interests, and a button to start the quiz.

A "Success Stories" section featuring testimonials from satisfied students, with placeholders for the testimonial text and student names.

A footer with links to the platform's blog, FAQ, privacy policy, and a "Contact Us" button that opens a modal window with a contact form and customer support information.

Include filler placeholder content for the video background, course cards, and testimonials. Embed the CSS styles within the <style> tag in the <head> section and place the JavaScript code within the <script> tag at the end of the <body> section.

The JavaScript code should handle the dynamic tagline in the hero section, rotating through the different taglines every 3 seconds.

Example output

```

&lt;!DOCTYPE html&gt;&lt;html lang=&quot;en&quot;&lt;head&lt;meta charset=&quot;UTF-8&quot;&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&lt;titleEduQuest - Online Learning Platform&lt;/title&lt;style/* CSS Styles */body {font-family: Arial, sans-serif;margin: 0;padding: 0;
        }
header {background-color: #333;color: #fff;padding: 20px;position: fixed;top: 0;left: 0;width: 100%;z-index: 999;
        }
nav ul {list-style: none;margin: 0;padding: 0;display: flex;justify-content: space-between;
        }
nav ul li a {color: #fff;text-decoration: none;padding: 10px;
        }
.hero {height: 100vh;display: flex;flex-direction: column;justify-content: center;align-items: center;text-align: center;position: relative;overflow: hidden;
        }
.hero video {position: absolute;top: 0;left: 0;width: 100%;height: 100%;object-fit: cover;z-index: -1;
        }
.hero h1 {font-size: 48px;margin-bottom: 20px;
        }
.hero p {font-size: 24px;margin-bottom: 40px;
        }
.hero button {padding: 10px 20px;font-size: 18px;background-color: #333;color: #fff;border: none;cursor: pointer;
        }
.featured-courses, .success-stories {padding: 40px;
        }
.course-card {background-color: #f5f5f5;padding: 20px;margin-bottom: 20px;
        }
.testimonial {background-color: #f5f5f5;padding: 20px;margin-bottom: 20px;
        }
footer {background-color: #333;color: #fff;padding: 20px;text-align: center;
        }
footer a {color: #fff;text-decoration: none;margin: 0 10px;
        }
footer button {padding: 10px 20px;font-size: 18px;background-color: #fff;color: #333;border: none;cursor: pointer;margin-top: 20px;
        }&lt;/style&lt;/head&lt;body&lt;header&lt;nav&lt;ul&lt;li&lt;a href=&quot;#&quot;Math&lt;/a&lt;/li&lt;li&lt;a href=&quot;#&quot;Science&lt;/a&lt;/li&lt;li&lt;a href=&quot;#&quot;Languages&lt;/a&lt;/li&lt;li&lt;a href=&quot;#&quot;Arts&lt;/a&lt;/li&lt;li&lt;input type=&quot;text&quot; placeholder=&quot;Search&quot;&lt;/li&lt;/ul&lt;/nav&lt;/header&lt;section class=&quot;hero&quot;&lt;video src=&quot;placeholder-video.mp4&quot; autoplay loop muted&lt;/video&lt;h1Welcome to EduQuest&lt;/h1&lt;p id=&quot;tagline&quot;Learn at your own pace&lt;/p&lt;buttonGet Started&lt;/button&lt;/section&lt;section class=&quot;featured-courses&quot;&lt;h2Featured Courses&lt;/h2&lt;div class=&quot;course-card&quot;&lt;img src=&quot;placeholder-course-image.jpg&quot; alt=&quot;Course Image&quot;&lt;h3Course Title&lt;/h3&lt;pInstructor: John Doe&lt;/p&lt;pCourse description goes here.&lt;/p&lt;/div&lt;div class=&quot;course-card&quot;&lt;img src=&quot;placeholder-course-image.jpg&quot; alt=&quot;Course Image&quot;&lt;h3Course Title&lt;/h3&lt;pInstructor: Jane Smith&lt;/p&lt;pCourse description goes here.&lt;/p&lt;/div&lt;/section&lt;section class=&quot;success-stories&quot;&lt;h2Success Stories&lt;/h2&lt;div class=&quot;testimonial&quot;&lt;p&quot;EduQuest helped me learn a new skill and advance my career. Highly recommended!&quot;&lt;/p&lt;pJohn Doe&lt;/p&lt;/div&lt;div class=&quot;testimonial&quot;&lt;p&quot;I never thought I could learn a new language online, but EduQuest made it possible. Thank you!&quot;&lt;/p&lt;pJane Smith&lt;/p&lt;/div&lt;/section&lt;footer&lt;a href=&quot;#&quot;Blog&lt;/a&lt;a href=&quot;#&quot;FAQ&lt;/a&lt;a href=&quot;#&quot;Privacy Policy&lt;/a&lt;buttonContact Us&lt;/button&lt;/footer&lt;script// JavaScript Codeconst taglines = [&quot;Learn at your own pace&quot;,&quot;Discover new passions&quot;,&quot;Expand your horizons&quot;
        ];
let index = 0;const taglineElement = document.getElementById(&quot;tagline&quot;);
function rotateTagline() {taglineElement.textContent = taglines[index];index = (index + 1) % taglines.length;
        }
setInterval(rotateTagline, 3000);&lt;/script&lt;/body&lt;/html
```

> This code creates a one-page website for "EduQuest" with the following features and sections:

1.  A fixed navigation bar with links to course categories (Math, Science, Languages, Arts) and a search bar.

1.  A hero section with a placeholder video background, a dynamic tagline that rotates between "Learn at your own pace," "Discover new passions," and "Expand your horizons" every 3 seconds, and a "Get Started" button.

1.  A featured courses section displaying course cards with placeholders for course images, titles, instructors, and descriptions.

1.  A "Success Stories" section featuring testimonials from satisfied students, with placeholders for the testimonial text and student names.

1.  A footer with links to the platform's blog, FAQ, privacy policy, and a "Contact Us" button.

> The CSS styles are embedded within the <style> tag in the <head> section, and the JavaScript code is placed within the <script> tag at the end of the <body> section.

> The JavaScript code handles the dynamic tagline in the hero section, rotating through the different taglines every 3 seconds using the setInterval function.

> Please note that the video background, course images, and testimonials are placeholders and should be replaced with actual content.

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=1000,
    temperature=0,
    system=&quot;Your task is to create a one-page website based on the given specifications, delivered as an HTML file with embedded JavaScript and CSS. The website should incorporate a variety of engaging and interactive design features, such as drop-down menus, dynamic text and content, clickable buttons, and more. Ensure that the design is visually appealing, responsive, and user-friendly. The HTML, CSS, and JavaScript code should be well-structured, efficiently organized, and properly commented for readability and maintainability.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Create a one-page website for an online learning platform called \&quot;EduQuest\&quot; with the following features and sections:\n\n1. A fixed navigation bar with links to course categories (Math, Science, Languages, Arts) and a search bar.\n\n2. A hero section with a video background showcasing students learning online, a dynamic tagline that rotates between \&quot;Learn at your own pace,\&quot; \&quot;Discover new passions,\&quot; and \&quot;Expand your horizons\&quot; every 3 seconds, and a \&quot;Get Started\&quot; button leading to a course catalog.\n\n3. A featured courses section displaying course cards with placeholders for course images, titles, instructors, and descriptions.\n\n5. A \&quot;Success Stories\&quot; section featuring testimonials from satisfied students, with placeholders for the testimonial text and student names.\n\n6. A footer with links to the platform&#x27;s blog, FAQ, privacy policy, and a \&quot;Contact Us\&quot; button.\n\nInclude filler placeholder content for the video background, course cards, and testimonials. Embed the CSS styles within the `&lt;style&gt;` tag in the `&lt;head&gt;` section and place the JavaScript code within the `&lt;script&gt;` tag at the end of the `&lt;body&gt;` section.\n\nThe JavaScript code should handle the dynamic tagline in the hero section, rotating through the different taglines every 3 seconds.&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 1000,
  temperature: 0,
  system: &quot;Your task is to create a one-page website based on the given specifications, delivered as an HTML file with embedded JavaScript and CSS. The website should incorporate a variety of engaging and interactive design features, such as drop-down menus, dynamic text and content, clickable buttons, and more. Ensure that the design is visually appealing, responsive, and user-friendly. The HTML, CSS, and JavaScript code should be well-structured, efficiently organized, and properly commented for readability and maintainability.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Create a one-page website for an online learning platform called \&quot;EduQuest\&quot; with the following features and sections:\n\n1. A fixed navigation bar with links to course categories (Math, Science, Languages, Arts) and a search bar.\n\n2. A hero section with a video background showcasing students learning online, a dynamic tagline that rotates between \&quot;Learn at your own pace,\&quot; \&quot;Discover new passions,\&quot; and \&quot;Expand your horizons\&quot; every 3 seconds, and a \&quot;Get Started\&quot; button leading to a course catalog.\n\n3. A featured courses section displaying course cards with placeholders for course images, titles, instructors, and descriptions.\n\n5. A \&quot;Success Stories\&quot; section featuring testimonials from satisfied students, with placeholders for the testimonial text and student names.\n\n6. A footer with links to the platform&#x27;s blog, FAQ, privacy policy, and a \&quot;Contact Us\&quot; button.\n\nInclude filler placeholder content for the video background, course cards, and testimonials. Embed the CSS styles within the `&lt;style&gt;` tag in the `&lt;head&gt;` section and place the JavaScript code within the `&lt;script&gt;` tag at the end of the `&lt;body&gt;` section.\n\nThe JavaScript code should handle the dynamic tagline in the hero section, rotating through the different taglines every 3 seconds.&quot;}
  ]
});
console.log(msg);
```

# Excel formula expert

Create Excel formulas based on user-described calculations or data manipulations.

prompt：

> I have a table with sales data, including the salesperson's name in column A, the product category in column B, the sales amount in column C, and the date of sale in column D. I want to calculate the total sales amount for each salesperson, but only for sales of products in the "Electronics" category that occurred in the month of January. Can you help me with the Excel formula to achieve this?

Example output：

> To calculate the total sales amount for each salesperson based on the given criteria, you can use the following nested Excel formula:

> =SUMPRODUCT((A2:A100=A2)(B2:B100="Electronics")(MONTH(D2:D100)=1)*(C2:C100))

> Explanation:

> The SUMPRODUCT function in Excel allows you to perform multiple calculations and conditions within a single formula. Here's how the formula works:

+   (A2:A100=A2) creates an array of TRUE/FALSE values indicating whether each cell in the range A2:A100 matches the salesperson's name in cell A2.

+   (B2:B100="Electronics") creates an array of TRUE/FALSE values indicating whether each cell in the range B2:B100 contains the product category "Electronics".

+   (MONTH(D2:D100)=1) creates an array of TRUE/FALSE values indicating whether each cell in the range D2:D100 represents a date in the month of January.

+   (C2:C100) is the range containing the sales amounts.

> The SUMPRODUCT function multiplies the corresponding elements of these arrays and then sums up the results. In this case, it multiplies the TRUE/FALSE values (1 for TRUE, 0 for FALSE) with the sales amounts, effectively summing only the sales that meet all the specified criteria.

> Tip: Make sure the ranges A2:A100, B2:B100, C2:C100, and D2:D100 cover all the relevant data in your table. If your data extends beyond row 100, adjust the ranges accordingly. Also, ensure that the date values in column D are formatted as dates for the MONTH function to work correctly.

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=1000,
    temperature=0,
    system=&quot;As an Excel Formula Expert, your task is to provide advanced Excel formulas that perform the complex calculations or data manipulations described by the user. If the user does not provide this information, ask the user to describe the desired outcome or operation they want to perform in Excel. Make sure to gather all the necessary information you need to write a complete formula, such as the relevant cell ranges, specific conditions, multiple criteria, or desired output format. Once you have a clear understanding of the user&#x27;s requirements, provide a detailed explanation of the Excel formula that would achieve the desired result. Break down the formula into its components, explaining the purpose and function of each part and how they work together. Additionally, provide any necessary context or tips for using the formula effectively within an Excel worksheet.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;I have a table with sales data, including the salesperson&#x27;s name in column A, the product category in column B, the sales amount in column C, and the date of sale in column D. I want to calculate the total sales amount for each salesperson, but only for sales of products in the \&quot;Electronics\&quot; category that occurred in the month of January. Can you help me with the Excel formula to achieve this?&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 1000,
  temperature: 0,
  system: &quot;As an Excel Formula Expert, your task is to provide advanced Excel formulas that perform the complex calculations or data manipulations described by the user. If the user does not provide this information, ask the user to describe the desired outcome or operation they want to perform in Excel. Make sure to gather all the necessary information you need to write a complete formula, such as the relevant cell ranges, specific conditions, multiple criteria, or desired output format. Once you have a clear understanding of the user&#x27;s requirements, provide a detailed explanation of the Excel formula that would achieve the desired result. Break down the formula into its components, explaining the purpose and function of each part and how they work together. Additionally, provide any necessary context or tips for using the formula effectively within an Excel worksheet.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;I have a table with sales data, including the salesperson&#x27;s name in column A, the product category in column B, the sales amount in column C, and the date of sale in column D. I want to calculate the total sales amount for each salesperson, but only for sales of products in the \&quot;Electronics\&quot; category that occurred in the month of January. Can you help me with the Excel formula to achieve this?&quot;}
  ]
});
console.log(msg);
```

# Google apps scripter

Generate Google Apps scripts to complete tasks based on user requirements.

prompt：

> Write me a Google apps script that will translate all text in a Google Slides presentation to Korean.

Example output

> Here's a Google Apps Script that translates all the text in a Google Slides presentation to Korean:

> JavaScript

```
function translateSlidesToKorean() {var presentation = SlidesApp.getActivePresentation();var slides = presentation.getSlides();
for (var i = 0; i &lt; slides.length; i++) {var slide = slides[i];var pageElements = slide.getPageElements();
for (var j = 0; j &lt; pageElements.length; j++) {var pageElement = pageElements[j];
if (pageElement.getPageElementType() === SlidesApp.PageElementType.SHAPE) {var shape = pageElement.asShape();if (shape.getText() !== null) {var text = shape.getText().asString();var translatedText = LanguageApp.translate(text, &#x27;&#x27;, &#x27;ko&#x27;);shape.getText().setText(translatedText);
        }
      } else if (pageElement.getPageElementType() === SlidesApp.PageElementType.TABLE) {var table = pageElement.asTable();var numRows = table.getNumRows();var numColumns = table.getNumColumns();
for (var row = 0; row &lt; numRows; row++) {for (var col = 0; col &lt; numColumns; col++) {var tableCell = table.getCell(row, col);var text = tableCell.getText().asString();var translatedText = LanguageApp.translate(text, &#x27;&#x27;, &#x27;ko&#x27;);tableCell.getText().setText(translatedText);
          }
        }
      }
    }
  }
}
```

> Here's how the script works:

1.  The translateSlidesToKorean() function retrieves the active Google Slides presentation using SlidesApp.getActivePresentation().

1.  It gets all the slides in the presentation using the getSlides() method.

1.  We iterate over each slide in the presentation.

1.  For each slide, we retrieve all the page elements (shapes, text boxes, tables, etc.) using the getPageElements() method.

1.  We iterate over each page element on the slide.

1.  We check the type of the page element using getPageElementType():

+   If the page element is a shape (SlidesApp.PageElementType.SHAPE), we get the text content of the shape using getText().asString(). We then translate the text to Korean using LanguageApp.translate(text, '', 'ko'), where '' represents the source language (auto-detect) and 'ko' represents the target language (Korean). Finally, we set the translated text back to the shape using setText().

+   If the page element is a table (SlidesApp.PageElementType.TABLE), we iterate over each cell in the table and translate the text within each cell similarly to how we handle shapes.

1.  The script continues to process all the page elements on each slide until all the text in the presentation has been translated to Korean.

> Note: This script assumes that you have the necessary permissions to access and modify the Google Slides presentation. Make sure you have the required access rights before running the script.

> Also, keep in mind that the translation is performed using the Google Translate API, so the quality of the translation may vary depending on the complexity of the text and the capabilities of the translation service.

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=2000,
    temperature=0,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Write me a Google apps script that will translate all text in a Google Slides presentation to Korean.&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 2000,
  temperature: 0,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Write me a Google apps script that will translate all text in a Google Slides presentation to Korean.&quot;}
  ]
});
console.log(msg);
```

# Python bug buster

Detect and fix bugs in Python code.

prompt：

> def calculate_average(nums):

> sum = 0

> for num in nums:

> sum += num

> average = sum / len(nums)

> return average

>

> numbers = [10, 20, 30, 40, 50]

> result = calculate_average(numbers)

> print("The average is:", results)

Example output

> Corrected Python code:

> Python

```
 def calculate_average(nums):  
     total = 0  
     for num in nums:  
         total += num  
     average = total / len(nums)  
     return average numbers = [10, 20, 30, 40, 50]  
 result = calculate_average(numbers)  
 print(&quot;The average is:&quot;, result)
```

> Explanation:

1.  Changed the variable name "sum" to "total" to avoid using the built-in Python function "sum()".

1.  Fixed the typo in the print statement, changing "results" to "result" to correctly reference the variable.

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=1000,
    temperature=0,
    system=&quot;Your task is to analyze the provided Python code snippet, identify any bugs or errors present, and provide a corrected version of the code that resolves these issues. Explain the problems you found in the original code and how your fixes address them. The corrected code should be functional, efficient, and adhere to best practices in Python programming.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;def calculate_average(nums):  \n    sum = 0  \n    for num in nums:  \n        sum += num  \n    average = sum / len(nums)  \n    return average  \n  \nnumbers = [10, 20, 30, 40, 50]  \nresult = calculate_average(numbers)  \nprint(\&quot;The average is:\&quot;, results)&quot;}
    ]
)
print(message.content)
```

TypeScrip：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 1000,
  temperature: 0,
  system: &quot;Your task is to analyze the provided Python code snippet, identify any bugs or errors present, and provide a corrected version of the code that resolves these issues. Explain the problems you found in the original code and how your fixes address them. The corrected code should be functional, efficient, and adhere to best practices in Python programming.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;def calculate_average(nums):  \n    sum = 0  \n    for num in nums:  \n        sum += num  \n    average = sum / len(nums)  \n    return average  \n  \nnumbers = [10, 20, 30, 40, 50]  \nresult = calculate_average(numbers)  \nprint(\&quot;The average is:\&quot;, results)&quot;}
  ]
});
console.log(msg);
```

# Time travel consultant

Help the user navigate hypothetical time travel scenarios and their implications.

prompt：

> Let's say I have a time machine and I travel back to the year 1900. While there, I accidentally prevent the invention of the airplane by the Wright brothers. What would be the potential implications of this action?

Example output

> That's an intriguing scenario! Let's explore the potential implications of preventing the invention of the airplane in 1900:

1.  Technological advancement: The invention of the airplane was a significant milestone in human history, paving the way for rapid globalization, modern warfare, and space exploration. Without it, the pace of technological advancement in fields like transportation, communication, and aerospace engineering might be significantly slower.

1.  Economic impact: The aviation industry has become a crucial part of the global economy, contributing to trade, tourism, and job creation. In a world without airplanes, the economy would likely be more localized, with a greater emphasis on ground and sea transportation.

1.  Cultural exchange: Air travel has facilitated the exchange of ideas, cultures, and people across the globe. Without airplanes, cultural exchange would be more limited, potentially leading to a less interconnected and diverse world.

1.  Butterfly effect: Removing such a significant invention from history could have far-reaching and unpredictable consequences. It might alter the course of major historical events, such as wars, migrations, and scientific discoveries, which could drastically change the world as we know it.

1.  Paradoxes: If your actions in the past prevent the invention of the airplane, it raises questions about the consistency of the timeline. Would you still have the knowledge and motivation to travel back in time in a world where airplanes were never invented?

> These are just a few potential implications to consider. It's important to remember that the consequences of altering past events can be complex, far-reaching, and often difficult to predict.

> What are your thoughts on this scenario? What other aspects or implications do you think are worth exploring?

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=1000,
    temperature=1,
    system=&quot;You are an AI assistant with expertise in physics, philosophy, and science fiction. Your task is to help users explore and understand the implications of hypothetical time travel scenarios. Provide detailed insights on the potential consequences, paradoxes, and ethical considerations involved in each specific scenario, while maintaining a friendly and engaging conversation.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Let&#x27;s say I have a time machine and I travel back to the year 1900. While there, I accidentally prevent the invention of the airplane by the Wright brothers. What would be the potential implications of this action?&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 1000,
  temperature: 1,
  system: &quot;You are an AI assistant with expertise in physics, philosophy, and science fiction. Your task is to help users explore and understand the implications of hypothetical time travel scenarios. Provide detailed insights on the potential consequences, paradoxes, and ethical considerations involved in each specific scenario, while maintaining a friendly and engaging conversation.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Let&#x27;s say I have a time machine and I travel back to the year 1900. While there, I accidentally prevent the invention of the airplane by the Wright brothers. What would be the potential implications of this action?&quot;}
  ]
});
console.log(msg);
```

# Storytelling sidekick

Collaboratively create engaging stories with the user, offering plot twists and character development.

prompt：

> Let's create a story about a young woman named Lila who discovers she has the power to control the weather. She lives in a small town where everyone knows each other.

Example output

> Great idea! Let's dive into Lila's story and explore how her newfound power affects her life and the town around her.

> Lila, a 22-year-old barista, has always been fascinated by the weather. One day, during a particularly strong thunderstorm, she discovers that she can control the elements with her mind. At first, she's thrilled and uses her power to help the town, ensuring perfect weather for important events like the annual harvest festival.

> However, as time goes on, some of the townspeople grow suspicious of Lila's uncanny ability to predict and control the weather. They start to whisper about her being a witch, and some even blame her for any adverse weather events that occur.

> To make matters worse, Lila's childhood friend, Tom, returns to town after years away. He's now a successful meteorologist and is determined to uncover the truth behind the town's unusual weather patterns. Lila must decide whether to trust Tom with her secret or risk losing his friendship.

> As tensions rise in the town, Lila learns that her powers come with a price. Each time she uses them, she experiences a physical and emotional toll. She must learn to control her abilities and find a way to balance her desire to help others with her own well-being.

> What do you think should happen next in the story? How could Lila's relationship with Tom evolve, and what challenges might she face as she tries to master her powers and win back the trust of the townspeople?

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=2000,
    temperature=1,
    system=&quot;You are an AI assistant with a passion for creative writing and storytelling. Your task is to collaborate with users to create engaging stories, offering imaginative plot twists and dynamic character development. Encourage the user to contribute their ideas and build upon them to create a captivating narrative.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Let&#x27;s create a story about a young woman named Lila who discovers she has the power to control the weather. She lives in a small town where everyone knows each other.&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 2000,
  temperature: 1,
  system: &quot;You are an AI assistant with a passion for creative writing and storytelling. Your task is to collaborate with users to create engaging stories, offering imaginative plot twists and dynamic character development. Encourage the user to contribute their ideas and build upon them to create a captivating narrative.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Let&#x27;s create a story about a young woman named Lila who discovers she has the power to control the weather. She lives in a small town where everyone knows each other.&quot;}
  ]
});
console.log(msg);
```

# Cite your sources

Get answers to questions about a document's content with relevant citations supporting the response.

prompt：

> Is Matterport doing well?

Example output

> [1] "We have recently experienced rapid growth. Our subscribers have grown approximately 49-fold from December 31, 2018 to December 31, 2022. Our revenue increased by approximately 22% to $136.1 million for the year ended December 31, 2022, from approximately $111.2 million for the year ended December 31, 2021."

> [2] "Our gross profit decreased by $8.1 million or 14%, to $51.8 million for the year ended December 31, 2022, from $60.0 million for the year ended December 31, 2021, primarily attributable to certain disruptive and incremental costs due to the global supply chain constraints in fiscal year 2022."

> [3] "Our net dollar expansion rate of 103% for the three months ended December 31, 2022 demonstrates the stickiness and growth potential of our platform."

> Answer:

> Matterport has experienced rapid subscriber and revenue growth in recent years. [1] However, their gross profit declined in 2022 due to supply chain issues. [2] Their net dollar expansion rate indicates strong subscriber retention and potential for continued growth. [3] Overall, despite the dip in gross profit, Matterport's subscriber and revenue growth, along with their platform's stickiness, suggest the company is doing relatively well.

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=1000,
    temperature=0,
    system=&quot;You are an expert research assistant. Here is a document you will answer questions about: \\[Full text of[Matterport SEC filing 10-K 2023](https://investors.matterport.com/node/9501/html), not pasted here for brevity]  \n  \nFirst, find the quotes from the document that are most relevant to answering the question, and then print them in numbered order. Quotes should be relatively short.  \n  \nIf there are no relevant quotes, write \&quot;No relevant quotes\&quot; instead.  \n  \nThen, answer the question, starting with \&quot;Answer:\&quot;. Do not include or reference quoted content verbatim in the answer. Don&#x27;t say \&quot;According to Quote [1]\&quot; when answering. Instead make references to quotes relevant to each section of the answer solely by adding their bracketed numbers at the end of relevant sentences.  \n  \nThus, the format of your overall response should look like what&#x27;s shown between the &lt;example&gt;&lt;/example&gt; tags. Make sure to follow the formatting and spacing exactly.  \n&lt;example&gt;  \nQuotes:  \n[1] \&quot;Company X reported revenue of $12 million in 2021.\&quot;  \n[2] \&quot;Almost 90% of revene came from widget sales, with gadget sales making up the remaining 10%.\&quot;  \n  \nAnswer:  \nCompany X earned $12 million. [1]  Almost 90% of it was from widget sales. [2]  \n&lt;/example&gt;  \n  \nIf the question cannot be answered by the document, say so.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Is Matterport doing well?&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 1000,
  temperature: 0,
  system: &quot;You are an expert research assistant. Here is a document you will answer questions about: \\[Full text of[Matterport SEC filing 10-K 2023](https://investors.matterport.com/node/9501/html), not pasted here for brevity]  \n  \nFirst, find the quotes from the document that are most relevant to answering the question, and then print them in numbered order. Quotes should be relatively short.  \n  \nIf there are no relevant quotes, write \&quot;No relevant quotes\&quot; instead.  \n  \nThen, answer the question, starting with \&quot;Answer:\&quot;. Do not include or reference quoted content verbatim in the answer. Don&#x27;t say \&quot;According to Quote [1]\&quot; when answering. Instead make references to quotes relevant to each section of the answer solely by adding their bracketed numbers at the end of relevant sentences.  \n  \nThus, the format of your overall response should look like what&#x27;s shown between the &lt;example&gt;&lt;/example&gt; tags. Make sure to follow the formatting and spacing exactly.  \n&lt;example&gt;  \nQuotes:  \n[1] \&quot;Company X reported revenue of $12 million in 2021.\&quot;  \n[2] \&quot;Almost 90% of revene came from widget sales, with gadget sales making up the remaining 10%.\&quot;  \n  \nAnswer:  \nCompany X earned $12 million. [1]  Almost 90% of it was from widget sales. [2]  \n&lt;/example&gt;  \n  \nIf the question cannot be answered by the document, say so.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Is Matterport doing well?&quot;}
  ]
});
console.log(msg);
```

# SQL sorcerer

Transform everyday language into SQL queries.

prompt：

> Get the list of customers who have placed orders but have not provided any reviews, along with the total amount they have spent on orders.

Example output

> SQL

```
SELECT c.first_name, c.last_name, SUM(o.total_amount) AS total_spent
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id
LEFT JOIN Reviews r ON c.customer_id = r.customer_idWHERE r.review_id IS NULLGROUP BY c.customer_id, c.first_name, c.last_name;
```

API request

Python：

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-3-opus-20240229&quot;,
    max_tokens=1000,
    temperature=0,
    system=&quot;Transform the following natural language requests into valid SQL queries. Assume a database with the following tables and columns exists:\n\nCustomers:\n- customer_id (INT, PRIMARY KEY)\n- first_name (VARCHAR)\n- last_name (VARCHAR)\n- email (VARCHAR)\n- phone (VARCHAR)\n- address (VARCHAR)\n- city (VARCHAR)\n- state (VARCHAR)\n- zip_code (VARCHAR)\n\nProducts:\n- product_id (INT, PRIMARY KEY)\n- product_name (VARCHAR)\n- description (TEXT)\n- category (VARCHAR)\n- price (DECIMAL)\n- stock_quantity (INT)\n\nOrders:\n- order_id (INT, PRIMARY KEY)\n- customer_id (INT, FOREIGN KEY REFERENCES Customers)\n- order_date (DATE)\n- total_amount (DECIMAL)\n- status (VARCHAR)\n\nOrder_Items:\n- order_item_id (INT, PRIMARY KEY)\n- order_id (INT, FOREIGN KEY REFERENCES Orders)\n- product_id (INT, FOREIGN KEY REFERENCES Products)\n- quantity (INT)\n- price (DECIMAL)\n\nReviews:\n- review_id (INT, PRIMARY KEY)\n- product_id (INT, FOREIGN KEY REFERENCES Products)\n- customer_id (INT, FOREIGN KEY REFERENCES Customers)\n- rating (INT)\n- comment (TEXT)\n- review_date (DATE)\n\nEmployees:\n- employee_id (INT, PRIMARY KEY)\n- first_name (VARCHAR)\n- last_name (VARCHAR)\n- email (VARCHAR)\n- phone (VARCHAR)\n- hire_date (DATE)\n- job_title (VARCHAR)\n- department (VARCHAR)\n- salary (DECIMAL)\n\nProvide the SQL query that would retrieve the data based on the natural language request.&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Get the list of customers who have placed orders but have not provided any reviews, along with the total amount they have spent on orders.&quot;}
    ]
)
print(message.content)
```

TypeScript：

```
import Anthropic from &quot;@anthropic-ai/sdk&quot;;

const anthropic = new Anthropic({
  apiKey: &quot;my_api_key&quot;, // defaults to process.env[&quot;ANTHROPIC_API_KEY&quot;]
});

const msg = await anthropic.messages.create({
  model: &quot;claude-3-opus-20240229&quot;,
  max_tokens: 1000,
  temperature: 0,
  system: &quot;Transform the following natural language requests into valid SQL queries. Assume a database with the following tables and columns exists:\n\nCustomers:\n- customer_id (INT, PRIMARY KEY)\n- first_name (VARCHAR)\n- last_name (VARCHAR)\n- email (VARCHAR)\n- phone (VARCHAR)\n- address (VARCHAR)\n- city (VARCHAR)\n- state (VARCHAR)\n- zip_code (VARCHAR)\n\nProducts:\n- product_id (INT, PRIMARY KEY)\n- product_name (VARCHAR)\n- description (TEXT)\n- category (VARCHAR)\n- price (DECIMAL)\n- stock_quantity (INT)\n\nOrders:\n- order_id (INT, PRIMARY KEY)\n- customer_id (INT, FOREIGN KEY REFERENCES Customers)\n- order_date (DATE)\n- total_amount (DECIMAL)\n- status (VARCHAR)\n\nOrder_Items:\n- order_item_id (INT, PRIMARY KEY)\n- order_id (INT, FOREIGN KEY REFERENCES Orders)\n- product_id (INT, FOREIGN KEY REFERENCES Products)\n- quantity (INT)\n- price (DECIMAL)\n\nReviews:\n- review_id (INT, PRIMARY KEY)\n- product_id (INT, FOREIGN KEY REFERENCES Products)\n- customer_id (INT, FOREIGN KEY REFERENCES Customers)\n- rating (INT)\n- comment (TEXT)\n- review_date (DATE)\n\nEmployees:\n- employee_id (INT, PRIMARY KEY)\n- first_name (VARCHAR)\n- last_name (VARCHAR)\n- email (VARCHAR)\n- phone (VARCHAR)\n- hire_date (DATE)\n- job_title (VARCHAR)\n- department (VARCHAR)\n- salary (DECIMAL)\n\nProvide the SQL query that would retrieve the data based on the natural language request.&quot;,
  messages: [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Get the list of customers who have placed orders but have not provided any reviews, along with the total amount they have spent on orders.&quot;}
  ]
});
console.log(msg);
```