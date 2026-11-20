
```python
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get("ANTHROPIC_API_KEY")
    api_key="my_api_key",
)
message = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=2000,
    temperature=0,
    messages=[
        {"role": "user", "content": "Write me a fully complete web app as a single HTML file. The app should contain a simple side-scrolling game where I use WASD to move around.  When moving around the world, occasionally the character/sprite will encounter words. When a word is encountered, the player must correctly type the word as fast as possible.The faster the word is successfully typed, the more point the player gets. We should have a counter in the top-right to keep track of points. Words should be random and highly variable to keep the game interesting. \n\nYou should make the website very aesthetic and use Tailwind."}
    ]
)
print(message.content)
```

TypeScript:

```typescript
import Anthropic from "@anthropic-ai/sdk";

const anthropic = new Anthropic({
  apiKey: "my_api_key", // defaults to process.env["ANTHROPIC_API_KEY"]
});

const msg = await anthropic.messages.create({
  model: "claude-3-opus-20240229",
  max_tokens: 2000,
  temperature: 0,
  messages: [
    {"role": "user", "content": "Write me a fully complete web app as a single HTML file. The app should contain a simple side-scrolling game where I use WASD to move around.  When moving around the world, occasionally the character/sprite will encounter words. When a word is encountered, the player must correctly type the word as fast as possible.The faster the word is successfully typed, the more point the player gets. We should have a counter in the top-right to keep track of points. Words should be random and highly variable to keep the game interesting. \n\nYou should make the website very aesthetic and use Tailwind."}
  ]
});
console.log(msg);
```

# Corporate clairvoyant

Extract insights, identify risks, and distill key information from long corporate reports into a single memo.

> Your task is to analyze the following `report`:
>
> `<report>`
>
> [Full text of Matterport SEC filing 10-K 2023, not pasted here for brevity]
>
> `</report>`
>
> Summarize this annual report in a concise and clear manner, and identify key market trends and takeaways. Output your findings as a short memo I can send to my team. The goal of the memo is to ensure my team stays up to date on how financial institutions are faring and qualitatively forecast and identify whether there are any operating and revenue risks to be expected in the coming quarter. Make sure to include all relevant details in your summary and analysis.

> To: Investment Team
>
> From: [Your Name]
>
> Date: March 4, 2024
>
> Subject: Matterport 2022 10-K Highlights and Financial Outlook
>
> Hi team,
>
> I have identified some key takeaways from Matterport's 2022 annual report:
>
> Financial Performance:
>
> +   Revenue increased 22% to $136.1M in 2022, driven by growth in subscription and services revenue
>
> +   However, gross margin decreased from 54% to 38% due to increased costs from product revenue and supply chain challenges
>
> +   Operating losses widened to $275.5M as the company invested heavily in R&D and sales & marketing to drive growth
>
> +   Ended 2022 with $476.9M in cash and investments to fund operations
>
> Business Highlights:
>
> +   Subscriber base grew 39% to over 701,000 at end of 2022. Spaces under management reached 9.2 million
>
> +   Expanded platform with launch of Matterport Axis motorized mount, Pro3 camera, and major updates to cloud software
>
> +   Completed acquisitions of Enview (AI for 3D spatial data) and VHT Studios (real estate marketing) to enhance offerings
>
> +   Signed notable partnerships with Burns & McDonnell and CGS to deliver Matterport's solutions to their enterprise customers
>
> Outlook:
>
> +   Matterport sees a massive market opportunity in digitizing the world's buildings and spaces (estimates $240B+ TAM)
>
> +   Focused on driving adoption with large enterprise customers across key verticals like real estate, AEC, facilities management, and more
>
> +   Will continue investing in R&D and international expansion to penetrate the largely untapped global building market
>
> +   Near-term growth and margins may be impacted by macroeconomic headwinds and supply chain issues
>
> +   However, healthy liquidity position and relatively sticky customer base provide downside protection
>
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

Include filler placeholder content for the video background, course cards, and testimonials. Embed the CSS styles within the `<style>` tag in the `<head>` section and place the JavaScript code within the `<script>` tag at the end of the `<body>` section.

The JavaScript code should handle the dynamic tagline in the hero section, rotating through the different taglines every 3 seconds.

Example output
