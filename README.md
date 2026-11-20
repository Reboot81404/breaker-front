# Turkish Classical Cipher Breaker – Frontend

A web-based interface developed as part of a computer engineering graduation project. It provides a user interface for breaking classical ciphers and experimenting with different encryption algorithms using the Turkish alphabet.

## Features

- **Cipher Breaking:** Sends encrypted text to the backend API and displays the decrypted text, detected cipher type, and estimated key.
- **Encryption Tools:** Provides separate interfaces for Substitution, Vigenère, Affine, and Playfair ciphers.
- **Search Modes:** Allows users to select between Fast, Medium, and Deep search modes.
- **Cipher Selection:** Supports individual algorithm selection or automatic analysis of Substitution and Vigenère ciphers.
- **Expert Mode:** Allows users to customize search parameters and n-gram scoring weights.

## Technologies

- Svelte
- JavaScript
- Vite
- CSS

The frontend communicates with the FastAPI backend to process cipher-breaking requests and display the results.

## Deployment

The frontend was previously deployed on Vercel and connected to a FastAPI backend. **The backend service is currently inactive**, so cipher-breaking functionality is not available in the live deployment.
