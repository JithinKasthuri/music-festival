# Tailwind CSS

## Project Setup

- Create a base folder for the project
- In project root folder, open a terminal and run - \
  `npm init` - Provide basic project details like name, version, author, etc., or \
  `npm init -y` - Keep the basic details to default value.\
  This will create a **<span style="color:green;">package.json</span>** file, which is which is a metadata file that defines the project and its dependencies.
- Create **index.html** inside src
- To install Tailwind - Go to Tailwind CSS Official website and follow the steps as per the project type. For example, if we are using any frameworks like React, it has to follow specific steps mentioned in the documentation. In this project we use the steps mentioned for **Tailwind CLI**. Below are the current steps (But check the official website and verify)
- `npm install tailwindcss @tailwindcss/cli`
- Create an **src** folder under the project root and create a file input.css inside it. Then add `@import "tailwindcss";`
  inside input.css. \
  This will have some reset css styles which will prevent the browser specific default styles. Apart from that, it will have tailwind css utility classes.
- Run the command - `npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --watch`. It runs the Tailwind CSS CLI tool to process your CSS and generate a final output file, while watching for changes.
- Instead of running this command again and again to build the CSS file, we can add it under the **scripts** section of `package.json`. For example:

  ```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "tw:build": "npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --watch"
  }
  ```

- Then we can run it with `npm run tw:build`. \
  This will create an **output.css** file under **dist** folder, which will contain all the Tailwind CSS utility classes and classes to reset browser default styles. \
  \
  In the output.css file, we need reset classes and those classes which are being used in our project. This can be configured in following ways -
  1. Adding **.gitignore** file and mentioning node_modules folder in it. All the Tailwind CSS utilities in output.css is coming from the tailwind module inside node_modules. Once we add it in .gitignore, it will not come in output.css.
  2. Modify input.css `@import "tailwindcss" source(none);` (Ignore the VS Code error) and rebuild. This will remove all the tailwind css utility classes except reset classes.To add css utility classes which are being used in our project, include `@source "./"` in input.css file (We are mentioning the path of html files where we tailwind css utility classes are used).
- **NOTE:** Before tailwind css version 4, there used to be a configuration file - **tailwind.config.js**. From version 4 onwards, it is not required.
- Additionally, we can install - `npm install -D prettier-plugin-tailwindcss`. Then create a `.prettierrc` file at the project root and add - \

  ```json
  {
    "plugins": ["prettier-plugin-tailwindcss"]
  }
  ```

  This will sort the Tailwind CSS classes in a pre-defined order.

## Tailwind CSS Layers: Base, Components, and Utilities

Tailwind CSS organizes styles into three main layers: **Base**, **Components**, and **Utilities**. Here's what each does:

---

### 1. Base Layer

**What it is:**  
The **Base** layer contains low-level styles like resets, global defaults, and typographic rules. Tailwind uses [Preflight](https://tailwindcss.com/docs/preflight), a modified Normalize.css, as its base.

**Example:**

```css
@tailwind base;
```

<pre lang="css"><code>@layer base { 
    h1 { font-size: 2rem; font-weight: bold; } 
    } </code>
</pre>

### 2. Components Layer

**What it is:**  
This layer is for custom, reusable UI components like buttons, cards, or modals. You can define groups of utility classes as reusable class names.
**Example:**

```css
@tailwind components;
```

<pre lang="css"><code>@layer components {
  .btn-primary {
    @apply bg-blue-500 text-white font-semibold py-2 px-4 rounded;
  }
} </code></pre>

### 3. Utilities Layer

**What it is:**  
TThis is the core of Tailwind CSS — the low-level, single-purpose utility classes like p-4, text-center, bg-red-500, etc.
**Example:**

```css
@tailwind utilities;
```

<pre lang="css"><code>
@layer utilities {
  .content-auto {
    content-visibility: auto;
  }
} </code>
</pre>

### Why These Layers Matter

Tailwind CSS loads styles in a specific order to ensure clarity and control in styling.

| **Layer**    | **Purpose**                    | **Order of Application** |
| ------------ | ------------------------------ | ------------------------ |
| `base`       | Global/reset styles            | 1️⃣ (applied first)       |
| `components` | Reusable styled elements       | 2️⃣                       |
| `utilities`  | Single-purpose utility classes | 3️⃣ (can override others) |

> ⚠️ **Tailwind applies these layers in this order**, so `utilities` will **override** `components` and `base` if there's a conflict.

## Adding External Fonts in Tailwind

We can add external fonts from google fonts.

- Go to google fonts official website and copy the import statement of font and paste it at the top of inpur.css file.
- We can add the font to a base/component layer or create a utility class for it. For example -

  ```css
  @layer base {
    body {
      font-family: "Quicksand", sans-serif;
    }
  }
  ```

  This will modify the font-family for body tag.

  ```css
  @theme {
    --font-quicksand: "Quicksand", sans-serif;
  }
  ```

  With this, we can use the utility class font-quicksand in our html files.

## How to avoid repeated styles with component layer?

Let's just say, we have multiple menu items in the top navbar -\

```css
<div class="relative flex h-full items-center p-4     cursor-pointer font-bold text-pink-200 hover:text-zinc-200 hover:bg-white/10 transition-colors ease-in-out">   <span>Home</span>
</div>
```

Instead of repeating the same utility class combo for other navbar items like Contact, About etc., we can create a component class in input.css -

```css
@layer components {
  .menu-item {
    @apply relative flex h-full cursor-pointer items-center p-4 font-bold text-pink-200 transition-colors ease-in-out hover:bg-white/10 hover:text-zinc-200;
  }
}
```

Now we can use `menu-item` class name directly in html.\
**NOTE:** Adding more custom styles can increase the bundle size.

## `group` selector in Tailwind CSS

A `group` selector is used when you want to style child elements based on the parent's state (like hover, focus, etc).

You use group class on the parent and then `group-<state>`: on the child.

Example - \

```css
<div class="menu-item group">
  <span>Tickets</span>

  <div class="hidden absolute top-full right-0 whitespace-nowrap bg-pink-500 rounded-b-md group-hover:block">
    <div class="p-4 font-bold hover:bg-white/5 hover:text-zinc-200 transition-colors ease-in-out cursor-pointer text-pink-200">
      <span>Single Day Ticket</span>
    </div>
    <div class="p-4 font-bold hover:bg-white/5 hover:text-zinc-200 transition-colors ease-in-out cursor-pointer text-pink-200">
      <span>7 Days Ticket</span>
    </div>
  </div>
</div>
```

When parent is hovered, child componets are made `display:block`.

## Tips/Notes in Tailwind CSS

- This - `calc(100vh - 200px)` is not valid in TailwindCSS, as the space before and after the `-` sign is not valid. We can either add `_` instead of space or remove the space and write it as `calc(100vh-200px)`
- We can modify the base styles like -

  ```css
  @layer base {
    body {
      font-family: var(--font-quicksand);
    }

    h1 {
      @apply text-6xl font-bold;
    }
    h2 {
      @apply text-5xl font-bold;
    }
    h3 {
      @apply text-4xl font-bold;
    }
    h4 {
      @apply text-3xl font-bold;
    }
    h5 {
      @apply text-2xl font-bold;
    }
    h6 {
      @apply text-xl font-bold;
    }
  }
  ```

  Now, for h1, h2, ..., h6 tags, we will have this styles by default.

- We can have dark and light theme with Tailwind, according to the system's theme. For example -
  ```html
  <main class="bg-zinc-200 dark:bg-zinc-900"></main>
  ```
  When switch to dark theme, background will change to zinc-200 color.
- We can make a Tailwind class important by just adding `!` before the class. Example - `!hidden`.
- By default Tailwind uses the `prefers-color-scheme` CSS media feature (A user indicates their preference of dark/light theme through an operating system setting), but you can also build sites that support toggling dark mode manually by overriding the dark variant. To implement dark/light theme via toggling button, we have to add `@custom-variant dark (&:where(.dark, .dark *));` in css file.
