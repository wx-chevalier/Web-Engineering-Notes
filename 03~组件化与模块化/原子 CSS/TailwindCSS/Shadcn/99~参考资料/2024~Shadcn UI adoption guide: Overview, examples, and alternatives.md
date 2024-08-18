# [Shadcn UI adoption guide: Overview, examples, and alternatives](https://blog.logrocket.com/shadcn-ui-adoption-guide/)

Shadcn UI takes a different approach from other UI and component libraries like Material UI and Chakra UI. Where those libraries offer access to components through their bundled packages, Shadcn UI instead allows you to download the source code for individual UI components into your codebase.

Also, according to its creator, Shadcn UI isn’t actually a component library. What exactly is it, then? We’ll explore that and more in this article!

We’ll learn about Shadcn UI, its features, its benefits, and why developers love and can’t seem to get enough of it. Along the way, we’ll build some UI interfaces with it. Let’s dive in!

## What is Shadcn UI?

[Shadcn UI](https://ui.shadcn.com/) is not actually a component library or UI framework. Instead, according to [the documentation](https://ui.shadcn.com/docs), it’s “a collection of reusable components that we can copy and paste into our apps.” This reusable component collection was created by [Shadcn](https://twitter.com/shadcn), who has also created great open source projects like [Taxonomy](https://vercel.com/templates/next.js/taxonomy), [Next.js for Drupal](https://next-drupal.org/), and [Reflexjs](https://reflexjs.org/).

Shadcn UI is built on top of Tailwind CSS and Radix UI and currently supports Next.js, Gatsby, Remix, Astro, Laravel, and Vite. However, there is an official [guide for manually integrating it with other technologies](https://ui.shadcn.com/docs/installation/manual).

Although Shadcn UI was released relatively recently in March 2023, it has quickly become one of the most [viral UI solutions](https://twitter.com/shadcn/status/1679494239305224192?t=LL5VtyMT1H1wwMaOjmUi3A&s=19) in the web development ecosystem. Everyone seems to be talking about and building with it, including the folks at Vercel. As of this writing, it [has nearly 60k GitHub stars](https://github.com/shadcn-ui/ui).

Shadcn UI grew so popular, so fast that [an unofficial Svelte extension was quickly released](https://twitter.com/huntabyte/status/1663320668887195648?t=gcZK91soVXh6WPK4JWNooQ&s=19) for it, which itself has over [4k GitHub stars](https://github.com/huntabyte/shadcn-svelte) as of this writing. The impact of the library has been so great that [its creator landed a job at Vercel](https://twitter.com/rauchg/status/1688947478652231680), meaning that Shadcn UI is now backed by Vercel.

## Why you should and shouldn’t use Shadcn UI

Even though it is a relatively new UI library, Shadcn UI has quickly risen in the ranks and is being adopted by several creators in the open source community and brands like Vercel. Its wide adoption is one great reason to use it — you’re likely to find tutorials, guides, and support from the community.

Here are a few other reasons you should use Shadcn UI:

- **Offers beautifully designed components**: One of Shadcn UI’s major selling points is its well-designed components. You’re sure to have an aesthetically pleasing website if you build it with Shadcn UI
- **Provides many components**: Shadcn UI offers various component options, ranging from basic UI elements like buttons and inputs to more complex components like toasts, dropdown menus, and navigation menus. This gives you a wide range of building blocks for your app
- **Built with Tailwind CSS**: Shadcn uses [Tailwind CSS](https://blog.logrocket.com/how-to-use-tailwind-css-react-vue-js/), a popular and [utility-first CSS framework](https://blog.logrocket.com/top-utility-first-css-frameworks/), for styling. This allows you to easily customize the components to match your application’s branding and design requirements

However, Shadcn UI is not for every developer or every project. Here’s why you shouldn’t use Shadcn UI:

- **Ownership and responsibility:** Since every component’s code lives in your codebase, you are responsible for maintaining the components and their dependencies and fixing any bugs or issues that may arise
- **Bundling and performance**: Any component you use will be included in your application’s bundle. This can potentially increase the overall bundle size and affect performance. You will have to manually use performance optimization techniques like code splitting to optimize the bundle size, which requires extra effort
- **Requires Tailwind CSS knowledge**: While it’s not a major issue, knowledge of Tailwind CSS is required to effectively use Shadcn UI

You should weigh these reasons to use or not use Shadcn UI, along with its pros and cons (which we’ll discuss below), before deciding whether or not it’s the right choice for your next project.

## Pros and cons of Shadcn UI

Here’s a rundown of the benefits you can enjoy by building with Shadcn UI:

- **Ease of use**: Anyone with basic knowledge of HTML, CSS, JavaScript, and a framework or two can quickly get started with Shadcn UI. Its components are easily accessed using either the copy and paste or the CLI installation method. I’ve used different component libraries like Material UI and Chakra UI, and Shadcn UI doesn’t fall short in that regard
- **Accessibility**: Shadcn UI’s components are fully accessible and adhere to Web Content Accessibility Guidelines (WCAG) standards. It supports screen readers, keyboard navigation, and other accessibility features. You can use Shadcn UI to create applications that are both beautiful and accessible to all users
- **Fine-grained control and extensibility**: Shadcn UI provides direct access to each component’s source code, so you can easily adjust the code to fit unique use cases and application needs. This ease of customization helps Shadcn UI stand out among other UI solutions and makes it a delight to work with. Easy access to component code also improves flexibility and makes it easier to scale and maintain applications

However, every technology has its disadvantages, including Shadcn UI. Here are some cons you should know before deciding to use Shadcn UI in future projects:

- **Manual installation of components**: Manually installing or copying every component you need can be a hassle. This extra step is not a big issue, but it can add more stress to the developer experience, particularly for those who are used to importing components from a package
- **Larger codebase**: Second, while having direct access to components’ code is beneficial in terms of modularity and extensibility, it also results in a larger codebase that requires more maintenance

Next, we’ll discuss a few general use cases for Shadcn UI.

## Use cases for Shadcn UI

While this is not an exhaustive list of potential use cases, you can apply Shadcn UI in the following ways:

- **Building visually appealing websites and applications**: Shadcn UI projects are well-designed, making it a great choice for building aesthetic UIs for websites and applications
- **Rapid prototyping and development**: You can significantly speed up development by building with Shadcn UI’s components, allowing you to focus on priority features
- **Building component libraries and design systems**: Shadcn UI is a highly customizable and extensible library, making it a great choice for building component libraries or design systems. You can easily modify the source code of Shadcn UI components to match your project’s needs and design guidelines

Let’s get to know Shadcn UI a little better by exploring several of its features.

## Shadcn UI features

Shadcn UI provides several features that you can leverage to enhance the development experience. Let’s take a closer look at features like its themes and theme editor, CLI, multiple components, and more.

### Themes, theme editor, and light/dark modes

Shadcn UI provides hand-picked themes that you can copy and paste into your applications. You can manually add theme tokens through the codebase, but you also achieve the same results using [Shadcn UI’s theme editor](https://ui.shadcn.com/themes).

The theme editor’s interface allows you to configure themes for properties such as color, border radius, and light or dark mode. You can also choose between two styles: `default` and `new-york`. The styles have unique components, animations, icons, and more.

The `default` style has larger input fields and uses [lucide-react](https://lucide.dev/) icons and [tailwindcss-animate](https://www.npmjs.com/package/tailwindcss-animate) for animations. The `new-york` style has smaller buttons, cards with shadows, and uses [Radix Icons](https://www.radix-ui.com/icons).

You can also easily create custom themes via Shadcn UI’s graphical interface. The editor outputs a piece of code containing your custom style definitions. Then, you simply copy the code and paste it into your application:

![Demo Of Using Shadcn Ui Graphical Interface To Customize An App Theme](https://blog.logrocket.com/wp-content/uploads/2023/11/Shadcn-UI-graphical-interface-customize-app-theme.gif)

Here’s a sample of the theme editor’s code output. It provides style tokens for both light mode and dark mode:

```css
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --card: 0 0% 100%;
    --card-foreground: 222.2 84% 4.9%;
    --popover: 0 0% 100%;
    --popover-foreground: 222.2 84% 4.9%;
    --primary: 221.2 83.2% 53.3%;
    --primary-foreground: 210 40% 98%;
    --secondary: 210 40% 96.1%;
    --secondary-foreground: 222.2 47.4% 11.2%;
    --muted: 210 40% 96.1%;
    --muted-foreground: 215.4 16.3% 46.9%;
    --accent: 210 40% 96.1%;
    --accent-foreground: 222.2 47.4% 11.2%;
    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 210 40% 98%;
    --border: 214.3 31.8% 91.4%;
    --input: 214.3 31.8% 91.4%;
    --ring: 221.2 83.2% 53.3%;
    --radius: 0.3rem;
  }

  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    --card: 222.2 84% 4.9%;
    --card-foreground: 210 40% 98%;
    --popover: 222.2 84% 4.9%;
    --popover-foreground: 210 40% 98%;
    --primary: 217.2 91.2% 59.8%;
    --primary-foreground: 222.2 47.4% 11.2%;
    --secondary: 217.2 32.6% 17.5%;
    --secondary-foreground: 210 40% 98%;
    --muted: 217.2 32.6% 17.5%;
    --muted-foreground: 215 20.2% 65.1%;
    --accent: 217.2 32.6% 17.5%;
    --accent-foreground: 210 40% 98%;
    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 210 40% 98%;
    --border: 217.2 32.6% 17.5%;
    --input: 217.2 32.6% 17.5%;
    --ring: 224.3 76.3% 48%;
  }
}
```

Shadcn UI supports dark mode for [Next.js](https://ui.shadcn.com/docs/dark-mode/next) and [Vite](https://ui.shadcn.com/docs/dark-mode/vite) applications. For Next.js applications, Shadcn UI uses [next-themes](https://github.com/pacocoursey/next-themes) for the dark mode toggling functionality. When a user toggles between light and dark mode, the application switches between the light and dark theme tokens.

### CLI

You can use [Shadcn UI’s CLI](https://ui.shadcn.com/docs/cli) to integrate the library with your applications, add dependencies, and apply the relevant `tailwind.config.js` configurations. You can also add UI components to your applications with the CLI.

You can either manually copy and paste the code for each component from the documentation or add them with the CLI. The CLI provides a superior developer experience and is one more feature that makes Shadcn UI to work with.

### Multiple components

As of this writing, [Shadcn UI has 48 components](https://ui.shadcn.com/docs/components/accordion), including `Accordion`, `Skeleton`, `Table`, and `Popover`. Instead of building components from scratch, you can save time by leveraging the Shadcn UI’s prebuilt components.

### Edit in v0

[v0](https://v0.dev/) is Vercel’s chatbot-like generative AI tool for building UI components. It uses Shadcn UI and Tailwind CSS to generate a component’s code based on the prompts it receives.

The **Edit in v0** feature allows you to open a Shadcn UI component’s code in the v0 app and use an AI prompt to fine-tune the component to match your needs. You need a v0 account to use this feature.

You can find the **Edit in v0** button on the documentation page for every Shadcn UI component.

### Blocks

[Blocks](https://ui.shadcn.com/blocks) are collections of Shadcn UI components that the creator combines to form ready-to-use pages. For example, instead of combining button, form, and input components to create an online form, you can use one of Shadcn UI’s form blocks.

Shadcn UI’s blocks are fully responsive and tailored to fit desktop, tablet, and mobile screen sizes. Integrating blocks is easy — all you need to do is copy the code for a specific block. Blocks can simplify the development process and increase your efficiency.

As of this writing, there are four form blocks and seven dashboard blocks.

### Lift mode

Lift mode is a new mode in blocks that allows you to lift individual components from a block and copy its code. This mode enables you to copy the code for smaller components that make up a block, like cards, buttons, and forms.

## Getting started with Shadcn UI

This tutorial will cover how to integrate Shadcn UI with Next.js. Refer to the [installation guide](https://ui.shadcn.com/docs/installation) for instructions on integrating with other frameworks.

To get started, create a new Next.js application by running the command below:

```bash
npx create-next-app@latest my-app --typescript --tailwind --eslint
```

Next, run the `init` command to initialize dependencies for a new project:

```bash
npx shadcn-ui@latest init
```

The CLI will prompt you to make some configurations. Here’s a sample of the configuration questions:

```plaintext
Would you like to use TypeScript (recommended)? no / yes
Which style would you like to use? › Default
Which color would you like to use as base color? › Slate
Where is your global CSS file? › › app/globals.css
Do you want to use CSS variables for colors? › no / yes
Where is your tailwind.config.js located? › tailwind.config.js
Configure the import alias for components: › @/components
Configure the import alias for utils: › @/lib/utils
Are you using React Server Components? › no / yes
```

That’s it! Now you can start adding components to your applications.

## Creating a button with Shadcn UI

Creating a button component with Shadcn UI is as easy as [copying its code from the button docs](https://ui.shadcn.com/docs/components/button) or adding it with the CLI. Run the command below to add it using the CLI:

```bash
npx shadcn-ui@latest add button
```

I prefer using the CLI because it automatically creates a `components` folder for me and makes the process more seamless. Once I have the component, all I need to do is export it from its folder, and bam! Here’s the code:

```javascript
import { Button } from "@/components/ui/button";

<Button variant="outline">Button</Button>;
```

The `Button` component has six variants: `default`, `destructive`, `outline`, `secondary`, `ghost`, and `link`.

## Creating a login form with Shadcn UI

I love that when it comes to forms, Shadcn UI goes a step further than just offering form components like `Input`, `Textarea`, `Checkbox`, and `RadioGroup`. It also provides [a `Form` component](https://ui.shadcn.com/docs/components/form), which is a wrapper around [react-hook-form](https://blog.logrocket.com/react-hook-form-complete-guide/).

Here are some of the things the `Form` component provides:

- A `<FormField />` component for building controlled form fields
- Support for validation with form validation libraries like [Valibot, Yup, and Zod](https://blog.logrocket.com/validating-structural-data-valibot/#comparing-valibots-design-approach-zod-yup)
- Error message handling

You can access the component by running the command below:

```bash
npx shadcn-ui@latest add form input
```

Next, set up the components for the form, like so:

```javascript
"use client";

import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import * as z from "zod";

import { Button } from "@/components/ui/button";
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";

const FormSchema = z.object({
  username: z.string().min(2, {
    message: "Username must be at least 2 characters.",
  }),
});

export function InputForm() {
  const form = useForm({ resolver: zodResolver(FormSchema) });

  function onSubmit(data) {
    <pre className="mt-2 w-[340px] rounded-md bg-slate-950 p-4">
      {JSON.stringify(data, null, 2)}{" "}
    </pre>;
  }
  return (
    <Form {...form}>
      {" "}
      <form onSubmit={form.handleSubmit(onSubmit)} className="w-2/3 space-y-6">
        {" "}
        <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              {" "}
              <FormLabel>Username</FormLabel> <FormControl>
                {" "}
                <Input placeholder="Input username" {...field} />{" "}
              </FormControl> <FormMessage />{" "}
            </FormItem>
          )}
        /> <Button type="submit">Submit</Button>{" "}
      </form>{" "}
    </Form>
  );
}
```

Here’s the online form built with the above code:

![Example Of An Online Form Built With Shadcn Ui With A Username Label Over An Input Field Over A Button](https://blog.logrocket.com/wp-content/uploads/2023/11/Example-online-form-built-Shadcn-UI.png)

## Shadcn UI vs. other UI and component libraries

While Shadcn UI has quickly become a top choice for building websites and applications, it’s not the only option available. The table below gives a breakdown of how Shadcn UI compares against other component libraries:

|                                           | Shadcn UI                                                                                                                                  | Material UI (MUI)                                                                                                                    | Chakra UI                                                                                                                        | Ant Design                                                                 |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| GitHub stars                              | 58.8k                                                                                                                                      | 91.8k                                                                                                                                | 36.7k                                                                                                                            | 90.4k                                                                      |
| Framework support                         | React, but there are Shadcn UI versions for [other frameworks](https://github.com/birobirobiro/awesome-shadcn-ui?tab=readme-ov-file#ports) | React, but there are MUI versions for [other frameworks](https://github.com/birobirobiro/awesome-shadcn-ui?tab=readme-ov-file#ports) | React, but there are Chakra UI versions for [Vue](https://vue.chakra-ui.com/) and [Svelte](https://chakra-ui-svelte.vercel.app/) | React, but there is a [Vue version](https://antdv.com/index) of Ant Design |
| Bundle size (minified + gzipped)          | NIL                                                                                                                                        | 93.7kb                                                                                                                               | 89kb                                                                                                                             | 429kb                                                                      |
| Number of components (as of this writing) | 48                                                                                                                                         | 66                                                                                                                                   | 62                                                                                                                               | 65                                                                         |
| Maturity                                  | Young and fast-growing library                                                                                                             | Established library                                                                                                                  | Young and fast growing library                                                                                                   | Established library                                                        |
| Best suited for                           | Small or personal projects                                                                                                                 | Small and large scale projects                                                                                                       | Small to large scale projects                                                                                                    | Small to large scale projects                                              |
| Design system                             |                                                                                                                                            | Google’s Material Design                                                                                                             | Chakra’s design system                                                                                                           | Ant Design system                                                          |
| Figma UI kit                              | No                                                                                                                                         | Yes                                                                                                                                  | Yes                                                                                                                              | Yes                                                                        |

## Conclusion

Shadcn UI is a breath of fresh air compared to existing component libraries, enabling developers to create beautiful user interfaces with a unique flair. It ticks the box in terms of user experience and development speed, while also giving devs fine-grained control over the components they use.

No library is ever a one-size-fits-all solution, but based on current trends, I expect Shadcn UI to become a top contender in the frontend ecosystem. It’s already being adopted by big players like Vercel.

For example, Vercel’s [v0 application](https://v0.dev/) is an AI-powered tool that uses Shadcn UI, Tailwind CSS, and text prompts to generate UI code that developers can copy and paste into their applications.

Shadcn UI is still a relatively new UI solution, and I hope to see more components added to it over time. What do you think about this new tool? Will you try it out in future projects? Are you already using it? Drop a comment — I’d love to know!
