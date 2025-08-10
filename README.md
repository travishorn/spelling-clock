# Spelling Clock

This project reimagines how we display time by using analog clocks as building blocks for digital digits. Instead of traditional LED displays or digital numbers, each digit (0-9) is composed of multiple analog clocks arranged in a grid pattern. The clock hands rotate to specific positions to form the shape of each digit when viewed together.

## Inspiration

This project was inspired by the innovative works of the artist studio **Humans since 1982**:

- **The ClockClock** - A mesmerizing installation where 24 analog clocks work together to display the current time in a unique way
- **A Million Times** - An art piece featuring 288 analog clocks that create dynamic patterns and animations

These works demonstrate how traditional analog clocks can be used in unexpected, creative ways to display information and create visual art.

## Features

- **Real-time Display**: Shows the current local time in 12-hour format
- **Analog Clock Digits**: Each digit is formed by multiple analog clocks
- **Smooth Transitions**: Clock hands smoothly rotate between digit changes
- **Responsive Design**: Adapts to different screen sizes
- **Modern UI**: Clean, minimalist interface with smooth animations

## Getting Started

### Prerequisites

- Node.js (version 18 or higher)

### Installation

1. Clone the repository:

```bash
git clone https://github.com/travishorn/spelling-clock
```

2. Change into the repository directory:

```bash
cd spelling-clock
```

3. Install dependencies:

```bash
npm install
```

4. Start the development server:

```bash
npm run dev
```

5. Open your browser and navigate to `http://localhost:5173`

### Building for Production

To create a production build:

```bash
npm run build
```

To preview the production build:

```bash
npm run preview
```

## Technology Stack

- **[SvelteKit](https://kit.svelte.dev/)** - Full-stack web framework
- **[TypeScript](https://www.typescriptlang.org/)** - Type-safe JavaScript
- **[Tailwind CSS](https://tailwindcss.com/)** - Utility-first CSS framework
- **[Vite](https://vitejs.dev/)** - Fast build tool and dev server

## Project Structure

```
spelling-clock/
├── src/
│   ├── lib/
│   │   ├── components/
│   │   │   ├── Clock.svelte      # Individual analog clock component
│   │   │   └── ClockDigit.svelte # Digit formed by multiple clocks
│   │   └── index.ts
│   ├── routes/
│   │   └── +page.svelte          # Main page with time logic
│   └── app.html
├── static/
└── package.json
```

## How It Works

1. **Time Calculation**: The app gets the current local time and converts it to 12-hour format
2. **Digit Parsing**: Each digit of the time is parsed individually:
   - Hours: No leading zero (1-12)
   - Minutes: With leading zero (00-59)
3. **Clock Positioning**: Each digit component calculates the required positions for its analog clocks
4. **Smooth Animation**: Clock hands smoothly rotate to their new positions with configurable transition times

## Contributing

Contributions are welcome! Feel free to:

- Report bugs
- Suggest new features
- Submit pull requests
- Improve documentation

## License

The MIT License (MIT)

Copyright © 2025 Travis Horn

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
