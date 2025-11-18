---

🚀 Building an AI-Powered Social Media Automation Platform for LinkedIn with Gemini, React, and Firebase
Introduction | Overview

The constant need for fresh, engaging content is a major challenge for professionals and businesses on platforms like LinkedIn. Manually crafting, scheduling, and analyzing posts is time-consuming and inefficient.
This blog outlines the solution: a comprehensive AI-powered social media automation platform designed to streamline the entire content lifecycle for LinkedIn. Built as a hackathon project, this platform leverages Google Gemini AI for content and image generation, React for a modern user interface, and Firebase for a scalable, serverless backend.
Audience: Developers and product managers interested in full-stack AI applications, modern web development (React/TypeScript), and integrating third-party APIs (LinkedIn OAuth/UGC API).
Final Outcome: By the end of this blog, you'll understand the core architecture and key technical achievements of building a full-featured social media automation tool that covers AI content creation, secure scheduling, and performance analytics.

---

Design
The platform's architecture follows a modern, full-stack, serverless approach, leveraging best-in-class tools for rapid development and scalability.
Key Architecture Decisions:

Frontend (React + TypeScript): A Single-Page Application (SPA) built with React 18 and TypeScript ensures a fast, type-safe, and maintainable user experience. Tailwind CSS provides rapid and responsive UI development.
Backend (Firebase + Google Cloud Run Proxy): We chose a serverless architecture with Firebase (Firestore for structured data, Storage for media files) for scalability and ease of deployment.

Backend Proxy (Cloud Run): A critical decision was implementing a custom backend proxy server (Flask or similar, deployed on Cloud Run) to manage two main challenges:
CORS Compliance: Directly calling the LinkedIn API from the browser is restricted by CORS policies. The proxy server acts as a compliant intermediary.
Secure OAuth Flow: It securely handles the LinkedIn OAuth 2.0 with PKCE flow and manages the secure storage and validation of user access tokens.

AI Integration (Google Gemini API): The Gemini API is integrated for both text generation and image generation (Imagen). A crucial design choice here was implementing a fallback mock system for development and demo purposes to handle potential API safety filter blocks or rate limitations.

This design ensures a Security-First approach (PKCE, token management) and a Scalable foundation (serverless components), while the AI-First strategy provides the core innovative value.

---

Prerequisites

To understand the technical depth of this project, a reader should be familiar with the following:
Languages: JavaScript/TypeScript, basic Python (for the backend proxy).
Frontend: React (hooks, context API) and modern web development concepts.
Backend: Basic understanding of Firebase (Firestore, Storage, Auth) and serverless functions (Cloud Run/Tasks).
APIs & Security: Working knowledge of REST APIs, OAuth 2.0 (specifically PKCE flow), and CORS.

Frameworks/Tools:
Node.js and Vite (for React development)
Docker (for containerizing the backend proxy)
Google Cloud SDK (for deployment)

---

Step-by-step Instructions (Core Feature Implementation)

1. Setting up Secure Authentication (LinkedIn OAuth with PKCE)

The first challenge is securing access to the user's LinkedIn profile.
Generate PKCE: On the frontend, generate a code_verifier (cryptographically secure random string) and a code_challenge (SHA256 hash of the verifier).
Initiate OAuth: Redirect the user to LinkedIn's authorization URL, including the code_challenge and a unique state.
Backend Proxy Exchange: The user is redirected back to the backend proxy with an authorization code. The proxy then securely exchanges this code for an access_token using the original code_verifier and stores the token in Firestore securely linked to the user's Firebase Auth UID.
Token Management: Implement logic to check for token expiration and validation on every API call, utilizing the proxy to manage the token lifecycle.

2. Integrating AI-Powered Content Creation (Gemini)

This is where the platform's intelligence shines.
Prompt Engineering: Implement Smart Prompts on the frontend, which are context-aware templates that guide the Gemini AI to generate LinkedIn-optimized content (professional, engaging tone).
TypeScript

// Example of AI-powered text generation const generated = await geminiService.generateText(   "Launch of our new AI product", // User's topic   { tone: "professional", target: "LinkedIn" } // Settings from Smart Prompts ); // Returns: "Excited to announce the launch of our revolutionary AI product..."

Visual Generation: Integrate the Gemini API for Image Generation based on the post's text, providing a professional style for the LinkedIn audience.
Safety Fallback: Implement the graceful degradation strategy: if the Gemini API's safety filters block professional content, the platform automatically serves a high-quality mock response for a smooth user experience.

3. Building the Scalable Post Scheduling System

Posts must be reliably executed at a future time.
Timezone Awareness: Use DateTime-local inputs on the frontend and store the scheduled time in UTC in Firestore.
Backend Tasking: Instead of relying on a running server, use Google Cloud Tasks. When a user schedules a post, the backend proxy creates a new Cloud Task to execute a specific API endpoint (e.g., /post/execute) at the scheduled time.
Live Posting: For immediate posts, the same endpoint is called directly, updating the post lifecycle in Firestore: Draft → Scheduled/Live.

4. File Upload and Media Management (Large Video Support)

Handling large media files (up to 200MB videos) is complex.
Firebase Storage: Use Firebase Storage for media file uploads. This is integrated with a Drag & Drop UI that provides resumable uploads and real-time progress callbacks to the user.
LinkedIn UGC API Flow: Once uploaded to Firebase Storage, the backend proxy follows the multi-step LinkedIn UGC API process:

Register the upload with LinkedIn to get an Upload URL.
Download the file from Firebase Storage.
Upload the media to the LinkedIn Upload URL.
Notify LinkedIn that the upload is complete.

---

Result / Demo

The final application is a modern, responsive single-page dashboard.
Analytics Dashboard: The homepage features an Analytics Dashboard with interactive Recharts-powered visualizations.
Data Aggregation: The platform fetches Impressions, Reactions, and Comments from LinkedIn and calculates key Engagement Rates.
Visualization Choice: Interactive Bar Charts and Line Graphs are used to show performance metrics and trends over 7, 14, and 28-day periods. This design allows users to quickly spot trends and correlate engagement with specific posting times or content types, driving a data-driven social media strategy.
Modern UI/UX: The application features a professional Dark Theme, real-time feedback via toast notifications, and an intuitive post creation panel that showcases the AI-generated text and images.

---

What's next?

This hackathon project provides a powerful foundation, but its potential can be expanded significantly:
Multi-Platform Expansion: Integrate with Twitter/X, Facebook/Instagram, and TikTok by adapting the content generation and API integration for each platform's unique content requirements.
Advanced Analytics: Implement features like A/B testing for different content variations and Optimal Posting Time recommendations using machine learning models trained on historical data.
Team Collaboration: Introduce Team Workspaces and Post Approval Workflows for agency or large-team use cases.
Automation Enhancements: Add Recurring Post Scheduling with Cron-like expressions and Auto-generated Content Series for evergreen topics.

---

Call to action

To learn more about Google Cloud services and to create impact for the work you do, get around to these steps right away:

Check out my portfolio webpage

Connect with me on LinkedIn

Register for Code Vipassana sessions

Join the meetup group Datapreneur Social

Sign up to become Google Cloud Innovator
