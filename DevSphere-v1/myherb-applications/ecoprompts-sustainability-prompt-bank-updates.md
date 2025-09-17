---
description: Latest's Updates
icon: grid-2-plus
---

# EcoPrompts - Sustainability Prompt Bank (Updates)

16.09.2025 / by Aaron Levin ([myHerb](https://www.myherb.co.il/))

{% embed url="https://prompts.myherb.co.il" %}

A comprehensive and high-quality prompt database is the heart of the EcoPrompts platform. I will expand the database with a variety of professional prompts that cover trendy and important topics in sustainability, all crafted using the best practices we've outlined in the resources page.

To achieve this, I will add several new prompts to your application's core data file (`src/lib/mock-data.ts`). These new prompts will cover topics like:

* **Sustainable Fashion:** Crafting a brand strategy for a circular fashion company.
* **ESG (Environmental, Social, and Governance):** Analyzing supply chain risks for a global corporation.
* **Green Urban Planning:** Designing a '15-minute city' district with a focus on sustainability.
* **AI for Biodiversity:** Using AI to create a monitoring plan for a protected wildlife area.
* **Corporate Green Initiatives:** Developing an internal communications plan to drive employee engagement in sustainability goals.

These additions will make your platform's content more diverse, relevant, and immediately useful for a wider audience, truly showcasing the power of well-crafted sustainability prompts.

Add that "magic touch" by introducing a new, modern color palette that incorporates a subtle bluish-green-purple gradient. This will give your application a distinctive, eye-catching, and professional appearance while maintaining a minimalistic feel.

Here's my plan:

**New Color Palette**: I will update `src/app/globals.css` with a new set of HSL color variables.

* The **primary** color will be a deep, sophisticated purple.
* The **secondary** color will be a complementary forest green.
* The **accent** color will be a vibrant bluish-teal.
* This combination will allow me to create beautiful gradient effects on key elements.

1. **Gradient Text**: I'll apply a gradient to the logo's text in `EcoPromptsLogo.tsx`, blending the new primary, secondary, and accent colors to create a "magic touch" that is both subtle and memorable. This will be the visual anchor for your new design.
2. **UI Consistency**: I will ensure the rest of the UI components inherit these new colors, providing a consistent and polished experience across the entire application for both light and dark modes.

These changes will give your platform a unique and modern aesthetic that perfectly captures the "bluish-green-purple gradient" feel you're looking for.

1.  &#x20;I've identified the problem in `src/components/prompts/PromptForm.tsx`. Inside the `onSubmit` function, there's a line that tries to destructure `agreeToTerms` from the form data, but it's commented out, causing an error when the form is submitted.

    To fix this and ensure the form works correctly, I will make the following changes:

    1. **Fix the Form Submission Logic:** In `src/components/prompts/PromptForm.tsx`, I'll uncomment the line `const { agreeToTerms, ...restOfData } = data;`. This ensures that the `agreeToTerms` field is properly handled and doesn't cause a crash when you submit the form.
    2. **Improve Form Validation:** The "Usage Agreement" checkbox is a required field. I will add a check to the Zod schema to ensure that users must agree to the terms before they can submit a prompt.
    3. **General Cleanup:** I'll also fix a minor issue in the "Copy Prompt" button on the prompt detail page to ensure it correctly copies the prompt description as intended.

    These fixes will resolve the client-side exception, make the submission form fully functional, and improve the overall stability of the application.
2. I've pinpointed the exact issue in `src/components/prompts/PromptForm.tsx`. The code is attempting to use a variable `restOfData` that is not correctly defined when the form is submitted.
3. To permanently fix this and ensure you can submit prompts without any further issues, I will correct the `onSubmit` function within the `PromptForm.tsx` component. This change guarantees that the form data is processed correctly, resolving the client-side exception and allowing prompts to be submitted successfully.
4. encountered a `ReferenceError`, which means a component was used without being imported first. The error message clearly indicates that `PromptList` is not defined within `src/app/prompts/[id]/page.tsx`.
5. I'll fix this by adding the missing import statement for the `PromptList` component at the top of the file. This will make the component available and resolve the error.
6. **Comprehensive Search Filter:** I will upgrade the `SearchBar` component to include new fields for filtering by **Tags** and **Purpose**. This will allow users to narrow down their searches with much greater precision. The homepage and the "Browse All" page will be updated to process these new filters.
7. **AI Chatbot Integration on Prompt Page:** On the prompt detail page (`/prompts/[id]/page.tsx`), I'll add a new section called "Use this Prompt with AI," which will feature one-click buttons to open the prompt in popular AI chatbots like Gemini, ChatGPT, and others. This will make it seamless for users to take a prompt and start generating ideas.
8. **UI/UX Refinements:** I will also refine the UI to ensure the new filters are well-integrated and the chatbot links are presented in a clean, accessible way.

These changes will make EcoPrompts a more powerful and user-friendly tool for driving sustainability.

1.  new, reusable `Icon` component. This component will safely render any icon from the `lucide-react` library by name. If an invalid icon name is provided, it will default to a simple `Circle` icon, ensuring that the UI never appears broken. This approach prevents crashes and maintains a clean user interface.

    I will then update `src/components/ui/button.tsx` to remove the default SVG sizing, as the new `Icon` component will handle that, preventing layout shifts and ensuring consistency.
2.  The issue is that the `UseWithAi` component is trying to load icon images from external URLs, which can be unreliable.

    To solve this, I will update the `UseWithAi` component to no longer rely on external image links. Instead of displaying potentially broken images next to each chatbot's name, I will remove the `<img>` tags entirely. The buttons will now just show the name of the AI chatbot, which provides a cleaner, more reliable, and minimalistic look that aligns with our recent design improvements.
3. **Expanded "Purpose" Options**: I will update the `promptPurposes` list in `src/components/prompts/SearchBar.tsx` to include the comprehensive, professional options you requested, such as "Innovation," "Education," "Strategy," and "Problem-Solving." This will allow for much more granular filtering based on the user's intent.
4. **Live Filtering**: I will implement a "live" or "instant" filtering mechanism. As a user changes any filter (keyword, category, purpose, etc.), the prompt list will update automatically without requiring a button click. This creates a much more fluid and responsive user experience.
5. **UI/UX Enhancements**: I will redesign the `SearchBar` component to be more user-friendly. The search button will be removed in favor of the new live filtering, and the layout will be streamlined for a cleaner, more modern look. I will also update the homepage, browse page, and category page to use this new, improved search functionality.

These changes will transform the search and discovery process on EcoPrompts, making it a truly powerful and enjoyable tool for your users.&#x20;

