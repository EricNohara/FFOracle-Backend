# Fantasy Football Assistant Manager Backend

### Contributors: Eric, Noah, Temi, Zac

## Table of Contents

- [Project Overview](#project-overview)
- [Project Purpose](#project-purpose)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Setup & Installation](#setup--installation)
- [API Endpoints](#api-endpoints)
- [Testing](#testing)

## Project Overview

FFOracle is a comprehensive fantasy football assistant manager application that helps users make data-driven decisions for their fantasy football leagues. 
The backend provides a robust RESTful API that powers AI-driven roster recommendations, player comparisons, league performance tracking, and secure payment processing.

**Frontend Repository:** https://github.com/EricNohara/Fantasy-Football-Assistant-Manager-Frontend  
**Updater Repository** https://github.com/EricNohara/FFOracle-Updater/
**Production:** Deployed on Azure App Service

## Features
- **Real-Time NFL Data** - Integration with NFLVerse and ESPN APIs for current player statistics
- **Automated Updates** - Weekly updates for player stats, team data, game schedules, and betting odds
- **Comprehensive Stats** - Season stats, weekly stats, defensive stats, betting lines, and injury reports
- **AI-driven Roster Recommendations** - Get personalized start/sit recommendations for entire rosters based on player stats, matchups, betting lines, and custom scoring settings
- **Player Comparison Tool** - Compare any two players with detailed AI analysis and recommendations
- **League Performance Tracking** - Track weekly performance, accuracy metrics, and roster optimization across multiple weeks

## Tech Stack

- **Backend Framework:** ASP.NET Core Web API (.NET 8)
- **Database:** Supabase (PostgreSQL)
- **Authentication:** Supabase JWT Authentication
- **Language:** C#
- **Version Control:** Git

## Setup & Installation

The backed and frontend code can be downloaded from this repository and  
https://github.com/EricNohara/Fantasy-Football-Assistant-Manager-Frontend  
respectively. On downloading, navigate to each solution directory from the command line and enter "dotnet restore" to install all necessary packages. Obtain your own API keys and accounts for Supabase, NFLVerse, OpenAI, and Stripe. Create the file "appsettings.json" with body:

```
{
  //supabase keys
  "Supabase": {
    "Url": ,
    "ServiceRoleKey":
  },
  //stripe keys
  "Stripe": {
    "PublishableKey": ,
    "SecretKey": ,
    "WebhookSecret":
  },
  "OpenAI": {
    "ApiKey":
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },

  //Indicates hosts from which requests can be accepted. Currently set to accept all.
  "AllowedHosts": "*"
  "Smtp": {
    "Host": ,
    "Port": ,
    "Username": ,
    "Password": ,
    "UseSSL": ,
    "UseStartTls": ,
    "SenderName": ,
    "SenderEmail": 
  }

}
```

Fill in all empty fields with your API keys.
In order to test Stripe webhooks, you will have to download the Stripe CLI. More information can be found here:
https://docs.stripe.com/stripe-cli  
After downloading, open the command line and run "stripe login". Follow the instructions that appear to link this machine to your Stripe account.

## Api Endpoints

### Authentication & User Management
| **Method** | **Endpoint**                    | **Description**                          | **Auth Required** |
| ---------- | ------------------------------- | ---------------------------------------- | ----------------- |
| `POST`     | `/api/Users/signup`             | Register a new user                      | No                |
| `POST`     | `/api/Users/login`              | Authenticate and receive JWT token       | No                |
| `POST`     | `/api/Users/request-password-reset` | Request password reset email         | No                |
| `POST`     | `/api/Users/reset-password`     | Reset password with token                | No                |
| `PUT`      | `/api/Users`                    | Update user information                  | Yes               |
| `DELETE`   | `/api/Users`                    | Delete user account                      | Yes               |

### AI-Powered Features
| **Method** | **Endpoint**                                  | **Description**                          | **Auth Required** | **Tokens Cost** |
| ---------- | --------------------------------------------- | ---------------------------------------- | ----------------- | --------------- |
| `GET`      | `/api/RosterPrediction?leagueId={id}`         | Get AI roster recommendations            | Yes               | 10 tokens       |
| `GET`      | `/api/PlayerComparison/{id1}/{id2}/{position}?leagueId={id}` | Compare two players | Yes         | 1 token         |

### League Management
| **Method** | **Endpoint**                    | **Description**                          | **Auth Required** |
| ---------- | ------------------------------- | ---------------------------------------- | ----------------- |
| `POST`     | `/api/GetUserData/create-league` | Create a new fantasy league             | Yes               |
| `GET`      | `/api/GetUserData/user-leagues` | Get all leagues for authenticated user   | Yes               |
| `GET`      | `/api/GetUserData/league/{id}`  | Get detailed league information          | Yes               |
| `PUT`      | `/api/UpdateUserLeague/roster-settings` | Update league roster settings      | Yes               |
| `PUT`      | `/api/UpdateUserLeague/scoring-settings` | Update league scoring settings    | Yes               |
| `POST`     | `/api/UpdateUserLeague/add-member` | Add player/defense to league roster | Yes               |
| `DELETE`   | `/api/UpdateUserLeague/remove-member` | Remove player/defense from roster | Yes               |
| `PUT`      | `/api/UpdateUserLeague/update-picks` | Update start/sit picks for players | Yes               |

### League Performance
| **Method** | **Endpoint**                               | **Description**                          | **Auth Required** |
| ---------- | ------------------------------------------ | ---------------------------------------- | ----------------- |
| `GET`      | `/api/LeaguePerformance/{leagueId}/week/{week}` | Get league performance metrics      | Yes               |

### NFL Data
| **Method** | **Endpoint**                    | **Description**                          | **Auth Required** |
| ---------- | ------------------------------- | ---------------------------------------- | ----------------- |
| `GET`      | `/api/GetPlayersByPosition?position={pos}` | Get players by position (QB, RB, WR, TE, K) | Yes     |
| `GET`      | `/api/ESPN/news/nfl`            | Get NFL news from ESPN                   | No                |
| `GET`      | `/api/ESPN/news/nfl/{playerId}` | Get news for specific player             | No                |

### Payments
| **Method** | **Endpoint**                           | **Description**                   | **Auth Required** |
| ---------- | -------------------------------------- | --------------------------------- | ----------------- |
| `POST`     | `/api/Stripe/create-payment-intent`    | Create Stripe payment intent      | No                |
| `POST`     | `/api/StripeWebhook/check-payment-completion` | Webhook for payment events | No (Stripe signature) |

### Internal/Admin (Auto-Update)
| **Method** | **Endpoint**                    | **Description**                          |
| ---------- | ------------------------------- | ---------------------------------------- |
| `POST`     | `/api/AutoUpdate/all`           | Run all weekly data updates              |
| `POST`     | `/api/Players/all`              | Update all NFL player data               |
| `POST`     | `/api/Team/all`                 | Update all NFL team data                 |
| `POST`     | `/api/GamesThisWeek/all`        | Update weekly game schedule              |


# Testing

The app has been deployed to the link https://fforacle.vercel.app. Testing of the features can be done so there. 
If looking to test the app locally: 
Most features of this test implementation can be tested using the **auto-generated Swagger UI**.

To start the backend:

```bash
dotnet run --launch-profile "https"
```

Once running, copy the **local URL** displayed in the console and open it in your browser — then append `/swagger` to the end.

```
https://localhost:5001/swagger
```

This will open the **Swagger testing interface**.

---

## Supabase Testing

1. In Swagger, select the **Supabase** endpoint.
2. Click **"Try it out"**.
3. In the _table_ field, enter the name of any table in the project database schema (e.g., `"Users"`).
4. Click **"Execute"**.

This will retrieve all entries from the given table.

> **Note:** The Supabase database is not yet fully populated.

---

## ChatGPT Testing

1. Select the **ChatGPT Test** endpoint.
2. Click **"Try it out"**.
3. Enter a quotation-enclosed string (e.g., `"Hello ChatGPT!"`).
4. Click **"Execute"**.

**Sample Query:**

```
{
  "message": "Return a json object with fields whoToStart and reasoning: {\"player_one\": {\"name\": \"Aaron Rodgers\", \"position\": \"QB\", \"team\": \"PIT\", \"completions\": 74, \"attempts\": 108, \"passing_yards\": 786, \"passing_tds\": 8, \"passing_interceptions\": 3, \"sacks_against\": 9, \"passing_air_yards\": 515, \"passing_first_downs\": 31, \"passing_epa\": 6.51129352118511, \"carries\": 9, \"rushing_yards\": 11, \"rushing_tds\": 0, \"fumbles\": 1, \"rushing_first_downs\": 1, \"rushing_epa\": -2.51797030409565, \"fantasy_points\": 60.54, \"fantasy_points_ppr\": 60.54}, \"player_two\": {\"name\": \"Daniel Jones\", \"position\": \"QB\", \"team\": \"IND\", \"completions\": 87, \"attempts\": 121, \"passing_yards\": 1078, \"passing_tds\": 4, \"passing_interceptions\": 2, \"sacks_against\": 4, \"passing_air_yards\": 1013, \"passing_first_downs\": 52, \"passing_epa\": 38.9130099857412, \"carries\": 18, \"rushing_yards\": 54, \"rushing_tds\": 3, \"fumbles\": 1, \"rushing_first_downs\": 8, \"rushing_epa\": 8.26075707325101, \"fantasy_points\": 78.52, \"fantasy_points_ppr\": 78.52}}"
}
```

The response will display ChatGPT’s reply to your input string.

---

## NFLVerse Testing

1. Select the **Players (POST)** endpoint.
2. Click **"Try it out"**, then **"Execute"**.
3. This retrieves all NFL player data from **NFLVerse** and stores it in the project **Supabase** database.
4. To confirm, use the Supabase testing steps above and enter the table name `"Players"`.

---

## Stripe Testing

To view **Stripe event logs**, stop the backend and set the environment to Development:

```bash
set ASPNETCORE_ENVIRONMENT=Development
```

This enables debug logs on the console.

> **Note:** Stripe testing requires both the backend and frontend to be running.

### 1. Start the Stripe listener

Open a terminal and enter:

```bash
stripe listen --forward-to https://localhost:<local port>/api/stripewebhook/check-payment-completion
```

This will register your machine as a temporary webhook endpoint.

### 2. Test the payment flow

From the frontend UI, enter the following **test card info**:

| **Number**          | **Expiration Date**   | **CVC** |
| ------------------- | --------------------- | ------- |
| 4111 1111 1111 1111 | any future month/year | 111     |

This is a **fake credit card** for testing only.

Press **"Pay $10"** to initiate a payment.  
You should see a `PaymentIntent succeeded` log in your backend console.  
Your Stripe dashboard should show a successful $10 test payment.
