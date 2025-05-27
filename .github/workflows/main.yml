import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
import psycopg2

# Spotify auth
sp = spotipy.Spotify(auth_manager=SpotifyClientCredentials(
    client_id="46e79f3178a3409ca4fb40615ccaefbc",
    client_secret="55f4ff9cbe0041d6a30e47e7fd566671"
))

# Supabase (PostgreSQL) connection
conn = psycopg2.connect(
    host="db.narfgkhutqbojyvtbfdo.supabase.co",
    dbname="postgres",
    user="postgres",
    password="ladder14636!",
    port="5432"
)
cursor = conn.cursor()

# Example: get tracks from a playlist
playlist_id = "37i9dQZF1DXcBWIGoYBM5M"  # e.g., Today's Top Hits
tracks = sp.playlist_tracks(playlist_id)["items"]

for item in tracks:
    track = item['track']
    audio_features = sp.audio_features(track['id'])[0]

    cursor.execute("""
        INSERT INTO Billboard Top 100 (
            artist, song, duration_ms, explicit, release_year, popularity,
            danceability, energy, key, loudness, mode,
            speechiness, acousticness, instrumentalness, liveness, valence, tempo, genre
        )
        VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)
    """, (
        track['artists'][0]['name'],
        track['name'],
        track['duration_ms'],
        track['explicit'],
        int(track['album']['release_date'][:4]),
        track['popularity'],
        audio_features['danceability'],
        audio_features['energy'],
        audio_features['key'],
        audio_features['loudness'],
        audio_features['mode'],
        audio_features['speechiness'],
        audio_features['acousticness'],
        audio_features['instrumentalness'],
        audio_features['liveness'],
        audio_features['valence'],
        audio_features['tempo']
    ))

conn.commit()
cursor.close()
conn.close()
