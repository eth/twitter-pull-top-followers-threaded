import tweepy
import json
from concurrent.futures import ThreadPoolExecutor

# Replace the placeholders with your own API keys and access tokens
consumer_key = 'your_consumer_key'
consumer_secret = 'your_consumer_secret'
access_token = 'your_access_token'
access_token_secret = 'your_access_token_secret'

def authenticate_twitter_api():
    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth, wait_on_rate_limit=True)
    return api

def get_follower_data(follower):
    return (follower.screen_name, follower.followers_count)

def get_ordered_followers(user_handle):
    api = authenticate_twitter_api()
    followers = []

    with ThreadPoolExecutor(max_workers=10) as executor:
        for follower in tweepy.Cursor(api.get_followers, screen_name=user_handle, count=200).items():
            future = executor.submit(get_follower_data, follower)
            followers.append(future.result())

    followers.sort(key=lambda x: x[1], reverse=True)
    return followers

user_handle = 'some_twitter_handle'  # Replace with the desired Twitter handle
ordered_followers = get_ordered_followers(user_handle)

with open("follower_count.json", "w") as f:
        json.dump(ordered_followers, f, indent=4)
