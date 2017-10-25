import spotipy
import sys
import pprint
import spotipy.util as util
import re


scope = 'playlist-modify-public'
username = 'client_id'


token = util.prompt_for_user_token(username, scope,
                                   client_id='client_id',
                                   client_secret='client_secret',
                                   redirect_uri='redirect_url')
'''
To use this script you need to register your app in Spotify API. After registration Spotify will provide auth details.
'''

sp = spotipy.Spotify(auth=token)


song_list = []

'''
Since vk.com music API is closed, songs were saved in simple text format copied from the web page. 
This text file is parsed by this script to get the list of songs. 

Acceptable format:

2:30
Tony Burns
Sanctions

3:30
Lindsey Ray
Brand New Day

3:30
The Pierces
Sticks And Stones
'''


with open('vk-songs', 'r', encoding="utf-8") as f:
    current_song = str()

    for line in f:
        line = line.strip(' \n')
        # print(line)
        if re.match('^[0-9]', line):
            song_list.append(current_song)
            current_song = ''
            continue
        if len(line) == 0:
            continue
        current_song = (current_song + ' - ' + line).strip(' -')


search_dict = {}
track_list = []

for song in song_list:
    result = sp.search(song)

    if len(result['tracks']['items']) == 0:
        search_dict[song] = ['Song not found']
        print(song)
        continue

    track_id = result['tracks']['items'][0]['id']
    track_name = result['tracks']['items'][0]['name']
    artist_name = result['tracks']['items'][0]['artists'][0]['name']

    search_dict[song] = [track_id, track_name, artist_name]
    track_list.append(track_id)


for track in track_list:
    results = sp.user_playlist_add_tracks(user='spotify_username', playlist_id='playlist_id', tracks=[track])


