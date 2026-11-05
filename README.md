# BookNook — Client Side

BookNook is an online library where readers can discover books, share their own collection, borrow available copies, and return them.

## Author

- **Name:** Md. Faruk Ahmed
- **GitHub:** [srfarukahmed](https://github.com/srfarukahmed)
- **Client Repository:** [booknook-client](https://github.com/srfarukahmed/booknook-client)
- **Live Website: https://booknook-faruk.vercel.app

## Features

- Responsive navigation with authentication-dependent links and an account dropdown.
- Three-slide Swiper banner with links to browse books.
- Six newest arrivals loaded through the library API.
- Book catalog with title/author search, category filters, and sorting.
- Login and registration forms with password validation and optional Google sign-in.
- Private routes that return users to their requested page after login.
- Add-book form with image upload, category, description, and copy count.
- Personal book management with prefilled editing and custom deletion confirmation.
- Borrow confirmation, fourteen-day due dates, and a personal borrowing table.
- Available-copy indicators and duplicate-borrow feedback.
- Toast notifications, loading states, empty states, and a custom 404 page.
- Persistent light/dark theme and layouts for mobile, tablet, and desktop.

## Technology Stack

| Technology | Purpose |
| --- | --- |
| Next.js App Router | Application framework and routing |
| TypeScript | Typed application development |
| React | User interface components |
| Tailwind CSS | Styling and responsive layouts |
| DaisyUI | UI components |
| Swiper.js | Home banner slider |
| BetterAuth | Authentication and sessions |
| MongoDB Adapter | Authentication data persistence |
| JWT Client Plugin | Tokens for protected API requests |
| Sonner | Toast notifications |
| Lucide React | Interface icons |

## Pages and Routes

| Route | Description | Access |
| --- | --- | --- |
| `/` | Home, new arrivals, and community sections | Public |
| `/books` | Searchable book catalog | Public |
| `/login` | Email/password and Google login | Public |
| `/register` | Account registration | Public |
| `/books/[id]` | Book details and borrowing | Private |
| `/add-book` | Add a book | Private |
| `/my-books` | Manage contributed books | Private |
| `/my-borrows` | View and return borrowed books | Private |
| `/profile` | Reader profile | Private |

The navigation and footer appear throughout the application.

## Folder Structure

```text
booknook-client/
├── app/
│   ├── api/auth/[...all]/
│   │   └── route.ts
│   ├── add-book/
│   ├── books/
│   │   ├── [id]/
│   │   └── page.tsx
│   ├── login/
│   ├── register/
│   ├── my-books/
│   ├── my-borrows/
│   ├── profile/
│   ├── error.tsx
│   ├── globals.css
│   ├── icon.svg
│   ├── layout.tsx
│   ├── loading.tsx
│   ├── not-found.tsx
│   └── page.tsx
├── components/
│   ├── auth-form.tsx
│   ├── book-form.tsx
│   ├── books.tsx
│   ├── home.tsx
│   ├── provider.tsx
│   ├── reading-pages.tsx
│   ├── shell.tsx
│   └── ui.tsx
├── lib/
│   ├── api.ts
│   ├── auth-client.ts
│   ├── seed.ts
│   └── types.ts
├── packages/
│   └── auth/
│       ├── src/index.ts
│       ├── package.json
│       └── tsconfig.json
├── public/
│   └── covers/
├── scripts/
│   ├── generate-covers.mjs
│   └── setup.mjs
├── .env.example
├── .gitignore
├── next.config.ts
├── package.json
├── package-lock.json
├── postcss.config.mjs
├── tsconfig.json
└── README.md
```

The Next.js authentication handler and `packages/auth` are included because authentication runs inside Next.js. The Express library API is maintained separately.

## Requirements

- Node.js 22.12 or newer.
- npm.
- A running BookNook Express API for connected mode.
- MongoDB Atlas or a replica set for BetterAuth persistence.
- Optional Google OAuth and imgbb credentials.

## Installation

Clone the client repository:

```bash
git clone https://github.com/srfarukahmed/booknook-client.git
cd booknook-client
npm ci
npm run setup
```

Configure the generated `.env.local` before starting the application.

## Environment Variables

```dotenv
MONGODB_URI=YOUR_MONGODB_ATLAS_CONNECTION_STRING
MONGODB_DB=booknook

BETTER_AUTH_URL=http://localhost:3005
BETTER_AUTH_SECRET=YOUR_AUTHENTICATION_SECRET
JWT_AUDIENCE=booknook-api
CLIENT_ORIGIN=http://localhost:3005

NEXT_PUBLIC_DEMO_MODE=false
NEXT_PUBLIC_AUTH_URL=http://localhost:3005
NEXT_PUBLIC_API_URL=http://localhost:5000

NEXT_PUBLIC_IMGBB_KEY=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
```

Keep `.env.local` private. Never expose database credentials, the authentication secret, or the Google client secret through `NEXT_PUBLIC_*` variables.

When connecting to an existing BookNook database, preserve its authentication secret so existing sessions and encrypted signing keys remain valid.

## Development

The prepared client uses port 3000 by default. To use port 3005, change the `--port 3000` arguments in the `dev` and `start` scripts to `--port 3005`.

Ensure the port matches the origins configured in `.env.local`.

Start the client:

```bash
npm run dev
```

With port 3005 configured, open:

```text
http://localhost:3005
```

Run the separate Express API at the address configured in `NEXT_PUBLIC_API_URL`.

## Production Build

Set production environment variables before building:

```bash
npm run build
npm start
```

Public environment variables are embedded during the build. Rebuild the application after changing them.

## Authentication

- Email/password login and registration use BetterAuth.
- Passwords require at least six characters, one uppercase letter, and one lowercase letter.
- Successful registration sends the reader to Login.
- Private routes preserve the requested destination for navigation after login.
- Session hydration completes before private-route redirects.
- The client obtains JWTs for protected Express API requests.
- Tokens are not persisted in localStorage in connected mode.
- Email verification and password recovery are intentionally disabled for the assignment.

### Google Sign-In

Configure a Google OAuth Web application with:

```text
Authorized origin:
http://localhost:3005

Redirect URI:
http://localhost:3005/api/auth/callback/google
```

Set both Google credentials and restart the application. Use the corresponding HTTPS URLs for deployment.

## Adding and Managing Books

The Add Book form includes:

- Book title.
- Author name.
- Category: Fiction, Science, History, Kids, or Others.
- Book cover upload or image URL.
- Short description.
- Total copies.
- Read-only contributor name and email.

Readers can edit their contributed books through a prefilled form. Deletion requires confirmation through a custom dialog.

## Borrowing and Returns

Readers can confirm borrowing from the book-details page and view their loans under My Borrows.

- Due dates are fourteen days after borrowing.
- Unavailable books show a disabled borrowing button.
- Duplicate borrowing produces an error message.
- Returning a book updates the borrowing list and availability.

The backend enforces ownership, availability, duplicate prevention, and inventory consistency.

## Image Uploads

Set `NEXT_PUBLIC_IMGBB_KEY` to upload JPG, PNG, or WebP cover images up to 4 MB.

Without an upload key, supply an HTTP(S) image URL. Connected mode stores image URLs rather than base64 image data.

## User Experience

- Consistent typography, buttons, spacing, and card dimensions.
- Three-column book grid on desktop.
- Responsive mobile navigation and page layouts.
- Toast-based feedback instead of browser alerts.
- Loading indicators and meaningful empty/error states.
- Route-specific page titles and a custom 404 page.
- Persistent light/dark theme.

## Optional Browser Preview

Set:

```dotenv
NEXT_PUBLIC_DEMO_MODE=true
```

This enables browser-only sample data and simulated authentication. It is intended for UI previews and does not provide production authentication or shared database persistence.

## Deployment

Deploy to a hosting platform that supports Next.js Node.js route handlers.

Configure MongoDB, authentication, API, Google, and upload environment variables as needed. Use HTTPS origins in production and register the deployed Google callback URL.

Update the **Live Website** link at the top of this README with the actual deployed URL before submitting the project.