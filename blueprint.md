# Technical Design Document: FastAPI + TMDb Recommender

## 1. Overview
A FastAPI application that integrates with The Movie Database (TMDb) to provide movie recommendations and details. The application will fetch a pool of movies (either from a user's watchlist or based on a specific genre), randomly select a defined number of movies from that pool, and return **one ultimate recommendation**. 

This document serves as a comprehensive blueprint to guide the development process as you learn FastAPI.

## 2. Architecture & Directory Structure
**Target Repository:** `~/Documents/Projects/tmbd-recommender`

The project will follow a modular structure, which is standard for FastAPI applications and helps separate concerns:

```text
tmbd-recommender/
├── main.py              # Entry point for the FastAPI application
├── requirements.txt     # Python dependencies (fastapi, uvicorn, httpx, pydantic-settings)
├── .env                 # Environment variables (TMDb API keys) - Added later
├── core/
│   └── config.py        # Configuration and environment variable loading
├── models/
│   └── schemas.py       # Pydantic models for API request/response validation
├── services/
│   └── tmdb_client.py   # Logic to interact with the TMDb external API (using httpx)
└── api/
    └── endpoints.py     # FastAPI route definitions
```

## 3. API Endpoints Design

### `GET /recommendation`
*   **Description**: Fetches a pool of movies, randomly selects `pool_size` movies from that pool, and then randomly selects and returns exactly **one** final recommendation.
*   **Query Parameters**: 
    *   `source` (string, required): Either `"watchlist"` or `"genre"`.
    *   `genre_id` (int, optional): Required if `source="genre"`.
    *   `pool_size` (int, default=5): The number of movies to initially filter down to before making the final random choice.
*   **TMDb Endpoints Used**: 
    *   If `source="watchlist"`: `GET /account/{account_id}/watchlist/movies`
    *   If `source="genre"`: `GET /discover/movie?with_genres={genre_id}`
*   **Response**: A JSON object containing the 1 final recommended movie (Title, TMDb ID, Release Date, Poster URL, Overview).

### `GET /movie/{movie_id}/summary`
*   **Description**: Retrieves detailed information about a specific movie.
*   **Path Parameters**: `movie_id` (int) - The TMDb movie ID.
*   **TMDb Endpoint Used**: `GET /movie/{movie_id}`
*   **Response**: Detailed summary, genres, runtime, user rating, and full overview.

### `GET /movie/{movie_id}/reviews`
*   **Description**: Retrieves user reviews for a specific movie.
*   **Path Parameters**: `movie_id` (int) - The TMDb movie ID.
*   **TMDb Endpoint Used**: `GET /movie/{movie_id}/reviews`
*   **Response**: A list of user reviews (author, content, rating).

## 4. Integration Details (TMDb)
*   **Authentication**: The app will eventually require a TMDb API Read Access Token. This will be stored securely in the `.env` file and loaded via `core/config.py`. *Note: Until you register for an API key, you can build the endpoints to return mock/hardcoded data.*
*   **HTTP Client**: The application will use the `httpx` library instead of `requests` to make asynchronous calls to TMDb, allowing FastAPI to remain highly performant.

## 5. Recommended Learning Path
Since your goal is to learn FastAPI by building this project, here is a recommended step-by-step implementation guide:

1.  **Environment Setup**: 
    *   Create the `tmbd-recommender` directory.
    *   Set up a Python virtual environment.
    *   Install core dependencies: `pip install fastapi uvicorn`.
2.  **Hello World**: 
    *   Create `main.py` with a simple `GET /` endpoint that returns `{"status": "ok"}`.
    *   Run the server using `uvicorn main:app --reload`.
3.  **Routing & Mock Endpoints**: 
    *   Set up `api/endpoints.py` and register the router in `main.py`.
    *   Create the three endpoints (`/recommendation`, `/movie/{id}/summary`, `/movie/{id}/reviews`) but have them return hardcoded mock dictionaries for now.
4.  **Pydantic Models**: 
    *   Create `models/schemas.py`.
    *   Define Pydantic classes for how you want your responses to look. Apply these as `response_model` in your route decorators.
5.  **External API Integration (httpx)**: 
    *   Install `httpx`.
    *   Get your TMDb API Key.
    *   Create `services/tmdb_client.py` with asynchronous functions to hit the real TMDb endpoints.
6.  **Business Logic**: 
    *   Update the `/recommendation` endpoint to call the `tmdb_client`.
    *   Implement Python's `random` module to select the `pool_size` and then pick the final 1 recommendation.
