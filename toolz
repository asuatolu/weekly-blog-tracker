import feedparser
import smtplib
from email.mime.text import MIMEText
from datetime import datetime, timedelta

# -------------------------
# CONFIG
# -------------------------
import os

EMAIL_FROM = "toluasua.ms@gmail.com"
EMAIL_TO = "toluasua.ms@gmail.com"
EMAIL_PASSWORD = os.environ.get("EMAIL_PASSWORD")

RSS_FEEDS = {
    "Neil Patel": "https://neilpatel.com/feed/",
    "Gary Vaynerchuk": "https://garyvaynerchuk.com/feed/",
    "Ann Handley": "https://annhandley.com/feed/",
    "WebFX": "https://www.webfx.com/blog/feed/",
    "Ignite Visibility": "https://ignitevisibility.com/feed/",
    "LYFE Marketing": "https://www.lyfemarketing.com/blog/feed/",
    "Disruptive Advertising": "https://disruptiveadvertising.com/feed/",
    "Column Five Media": "https://www.columnfivemedia.com/feed/",
    "SmartSites": "https://www.smartsites.com/blog/feed/",
    "Uplers": "https://www.uplers.com/blog/feed/"
}

# Only fetch posts from the last 7 days
DAYS_BACK = 7
cutoff_date = datetime.now() - timedelta(days=DAYS_BACK)

# -------------------------
# FETCH POSTS
# -------------------------
content = []

for name, feed_url in RSS_FEEDS.items():
    feed = feedparser.parse(feed_url)
    for entry in feed.entries:
        if hasattr(entry, "published_parsed"):
            published = datetime(*entry.published_parsed[:6])
            if published >= cutoff_date:
                content.append(
                    f"<li><strong>{name}</strong>: "
                    f"<a href='{entry.link}' target='_blank'>{entry.title}</a></li>"
                )

# -------------------------
# EMAIL BODY
# -------------------------
if not content:
    body = "<p>No new blog posts published this week.</p>"
else:
    body = "<h2>Weekly Marketing Blog Updates</h2><ul>" + "".join(content) + "</ul>"

msg = MIMEText(body, "html")
msg["Subject"] = "Weekly Competitor Blog Digest"
msg["From"] = EMAIL_FROM
msg["To"] = EMAIL_TO

# -------------------------
# SEND EMAIL
# -------------------------
with smtplib.SMTP_SSL("smtp.gmail.com", 465) as server:
    server.login(EMAIL_FROM, EMAIL_PASSWORD)
    server.send_message(msg)

print("Email sent successfully.")
