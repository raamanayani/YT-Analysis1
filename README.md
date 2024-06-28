# WEB SCARPPING 
# Overview
This project involves extracting and analyzing data from various websites using web scraping techniques. The primary focus is on gathering YouTube channel statistics for top influencers and visualizing the data for insightful analysis.

# Key Features
> API Integration: Utilizes the YouTube Data API to gather statistics and playlist information for top influencers such as PewDiePie, Emma Chamberlain, Sandeep Maheshwari, and TheOdd1sOut.
> Data Processing: Efficiently processes and structures the scraped data to ensure accuracy and relevance.
> Visualization: Implements advanced data visualization techniques using Seaborn and Matplotlib to present the collected data in an insightful and visually appealing manner.
> Error Handling: Employs robust error handling mechanisms to manage API requests and ensure seamless data retrieval.
> Python Programming: Leverages Python's powerful libraries and frameworks, such as Pandas, Seaborn, and Matplotlib, to streamline the scraping and analysis process.
# Table of Contents:
Installation
Usage
Project Structure
API Key Setup
Example
Contributing
License
Installation
Clone the repository:
bash
Copy code
git clone https://github.com/yourusername/web-scraping-project.git
cd web-scraping-project
Create a virtual environment:
bash
Copy code
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
Install the required dependencies:
bash
Copy code
pip install -r requirements.txt
Usage
Set up your YouTube API key:
Follow the instructions in the API Key Setup section.

Run the main script:

bash
Copy code
python main.py
View the output:
The results will be displayed in the console and visualized using Matplotlib.
# Project Structure
css
Copy code
web-scraping-project/
│
├── data/
│   ├── processed_data.csv
│
├── src/
│   ├── get_channel_stats.py
│   ├── visualize_data.py
│
├── main.py
├── requirements.txt
├── README.md
├── .gitignore
# API Key Setup
Go to the Google Cloud Console.
Create a new project or select an existing one.
Navigate to the API & Services section.
Enable the YouTube Data API v3.
Create an API key and add it to your environment variables:
export YOUTUBE_API_KEY='AIzaSyDH9YEC3N7COcim1arbbSRBrLP01nXsI44' 
> Example
Here's a brief example of how the script works:

> Fetching Channel Stats:
The script fetches channel statistics such as the number of subscribers, views, and total videos using the YouTube Data API.

> Visualizing Data:
The collected data is visualized using Seaborn and Matplotlib for easy analysis and insights.

python
Copy code
from googleapiclient.discovery import build
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Function to fetch channel stats
def get_channel_stats(youtube, channel_ids):
    all_data = []
    request = youtube.channels().list(
        part='snippet,contentDetails,statistics',
        id=','.join(channel_ids))
    response = request.execute()
    for item in response.get('items', []):
        data = {
            'Channel_name': item['snippet']['title'],
            'Subscribers': int(item['statistics']['subscriberCount']),
            'Views': int(item['statistics']['viewCount']),
            'Total_videos': int(item['statistics']['videoCount']),
            'Playlist_id': item['contentDetails']['relatedPlaylists']['uploads']
        }
        all_data.append(data)
    return all_data

# Initialize YouTube API client
api_key = 'AIzaSyDH9YEC3N7COcim1arbbSRBrLP01nXsI44'
youtube = build('youtube', 'v3', developerKey=api_key)

# List of channel IDs
channel_ids = [
    'UC-lHJZR3Gqxm24_Vd_AJ5Yw',  # PewDiePie
    'UC78cxCAcp7JfQPgKxYdyGrg',  # Emma Chamberlain
    'UCWqGdJ9eJ4Jibh4UZaNRlgA',  # Sandeep Maheshwari
    'UCo8bcnLyZH8tBIH9V1mLgqQ'   # TheOdd1sOut
]
Later the data has been visualised under various categories to fetch the views count and total number of videos using Seaborn 
# Function to get video IDS 
def get_video_ids(youtube, playlist_id):

    request = youtube.playlistItems().list(
                 part='contentDetails',
                 playlistId = playlist_id,
                 maxResults = 50)
    response = request.execute()

    video_ids = []

    for i in range(len(response['items'])):
        video_ids.append(response['items'][i]['contentDetails']['videoId'])

    next_page_token = response.get('nextPageToken')
    more_pages = True

    while more_pages:
        if next_page_token is None:
            more_pages = False
        else:
            request = youtube.playlistItems().list(
                        part = 'contentDetails',
                        playlistId = playlist_id,
                        maxResults=50,
                        pageToken = next_page_token)
            response = request.execute()
            for i in range(len(response['items'])):
                video_ids.append(response['items'][i]['contentDetails']['videoId'])
            next_page_token = response.get('nextPageToken')

    return video_ids
# Function to get video details 
def get_video_details(youtube, video_ids):
    all_video_stats = []
    for i in range(0, len(video_ids), 50):
        request = youtube.videos().list(
            part='snippet,statistics',
            id=','.join(video_ids[i:i+50])
        )
        response = request.execute()

        for video in response['items']:
            video_stats = dict(
                Title=video['snippet']['title'],
                Published_date=video['snippet']['publishedAt'],
                Views=video['statistics'].get('viewCount', 0),
                Likes=video['statistics'].get('likeCount', 0),
                Dislikes=video['statistics'].get('dislikeCount', 0),
                Comments=video['statistics'].get('commentCount', 0)
            )
            all_video_stats.append(video_stats)

    return  all_video_stats
This wa later visualised according to the requirements of getting to know the Top 10 videos of PEWDIEPIE channel using Seaborn 
 After this it was visualised again and sorted according to videos per month of PEWDIEPIE channel alone using Seaborn   
