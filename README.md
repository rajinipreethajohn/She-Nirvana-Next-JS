Project Plan: The She Nirvana - Women's Weekly Newsletter Web App
Database Schema (SQL)
Let's start with the necessary database schema:
sql-- Create users table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  subscription_status VARCHAR(20) DEFAULT 'free',
  stripe_customer_id VARCHAR(255)
);

-- Create newsletters table
CREATE TABLE newsletters (
  id SERIAL PRIMARY KEY,
  week_start DATE NOT NULL,
  week_end DATE NOT NULL,
  title VARCHAR(255) NOT NULL,
  intro_text TEXT,
  conclusion_text TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create content_sections table
CREATE TABLE content_sections (
  id SERIAL PRIMARY KEY,
  newsletter_id INTEGER REFERENCES newsletters(id) ON DELETE CASCADE,
  section_type VARCHAR(50) NOT NULL, -- 'celestial_insights', 'sacred_wellness', 'restful_rhythms', 'financial_wisdom', 'meditation_focus', 'sacred_affirmation'
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  emoji VARCHAR(10),
  premium_only BOOLEAN DEFAULT FALSE,
  display_order INTEGER NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create subscriptions table
CREATE TABLE subscriptions (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  stripe_subscription_id VARCHAR(255),
  plan_type VARCHAR(50) NOT NULL, -- 'free', 'premium', 'annual'
  status VARCHAR(50) NOT NULL, -- 'active', 'cancelled', 'past_due'
  current_period_start TIMESTAMP WITH TIME ZONE,
  current_period_end TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create tarot_readings table (for Daily Tarot Reading feature)
CREATE TABLE tarot_readings (
  id SERIAL PRIMARY KEY,
  date DATE NOT NULL,
  card_name VARCHAR(100) NOT NULL,
  image_url VARCHAR(255),
  interpretation TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create email_subscribers table (for non-user subscribers)
CREATE TABLE email_subscribers (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  first_name VARCHAR(100),
  subscribed_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  unsubscribed_at TIMESTAMP WITH TIME ZONE,
  is_active BOOLEAN DEFAULT TRUE
);
Implementation Plan
Phase 1: Basic Implementation
Step 1.1: Project Setup and Basic Components

Initialize Next.js project with TypeScript
Set up Supabase for database and authentication
Install necessary dependencies (tailwindcss, stripe, etc.)
Create basic folder structure and layout components
Implement responsive design with TailwindCSS

Step 1.2: Database Connection and Authentication

Connect to Supabase database
Set up authentication with Supabase Auth
Create login/signup components
Implement protected routes for subscribers

Step 1.3: Homepage and Landing Page

Create homepage with parchment paper background
Implement interactive circle layout with central hub
Build newsletter preview component
Add email subscription form for free users

Step 1.4: Test Phase 1

Test authentication flow
Test responsive design across devices
Verify database connections
Check that newsletter preview works correctly

Phase 2: Advanced Implementation
Step 2.1: Newsletter Content Pages

Build dynamic newsletter content pages
Create detailed section components for:

Celestial Insights
Sacred Wellness
Restful Rhythms
Financial Wisdom
Meditation Focus
Sacred Affirmation


Implement content fetching from database
Add premium content markers

Step 2.2: Stripe Integration

Set up Stripe account and products
Implement subscription plans (monthly/annual)
Create checkout and payment flow
Integrate webhook handling for subscription events

Step 2.3: User Dashboard

Build user profile page
Create subscription management interface
Implement settings and preferences
Add newsletter history view

Step 2.4: Test Phase 2

Test Stripe integration and subscription flow
Verify premium content restrictions
Test user dashboard functionality
Check performance with larger content sets

Phase 3: Full Implementation
Step 3.1: Daily Tarot Reading Feature

Create tarot card database
Implement daily card selection algorithm
Build interactive tarot card component
Add personalized interpretation functionality

Step 3.2: Advanced Personalization

Implement user preferences for content
Create personalized recommendations
Add favorites and bookmarking system
Implement notification system for new newsletters

Step 3.3: Content Management System

Build admin dashboard for content creation
Create WYSIWYG editor for newsletter sections
Implement scheduling and publishing workflow
Add content analytics and metrics

Step 3.4: Test Phase 3

Test tarot reading functionality
Verify personalization features
Test admin content management
Check notification system

Phase 4: Bug Fixes and Production
Step 4.1: Performance Optimization

Implement server-side rendering for SEO
Add caching for frequently accessed content
Optimize database queries
Implement image optimization

Step 4.2: Security Enhancements

Add CSRF protection
Implement rate limiting
Set up security headers
Add data encryption for sensitive information

Step 4.3: Final Testing

Conduct comprehensive integration testing
Perform security audit
Test across multiple browsers and devices
Verify all user flows and edge cases

Step 4.4: Deployment and Monitoring

Set up production environment
Configure CI/CD pipeline
Implement logging and monitoring
Deploy to production
