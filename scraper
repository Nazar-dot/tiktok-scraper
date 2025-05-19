from flask import Flask, request, jsonify
import requests
from bs4 import BeautifulSoup

app = Flask(__name__)

HEADERS = {
    'User-Agent': 'Mozilla/5.0'
}

@app.route('/scrape', methods=['POST'])
def scrape_tiktok():
    data = request.get_json()
    video_url = data.get("video_link")
    
    try:
        response = requests.get(video_url, headers=HEADERS)
        soup = BeautifulSoup(response.text, 'html.parser')

        like_tag = soup.find("strong", {"data-e2e": "like-count"})
        likes = like_tag.get_text(strip=True) if like_tag else "0"

        views_tag = soup.find("strong", {"data-e2e": "video-views"})
        views = views_tag.get_text(strip=True) if views_tag else "0"
        
        return jsonify({
            "view_count": views,
            "likes_count": likes
        })

    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
