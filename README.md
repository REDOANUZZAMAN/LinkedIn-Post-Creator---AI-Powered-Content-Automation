# 📱 LinkedIn Post Creator - AI-Powered Content Automation

An intelligent n8n workflow that automatically creates engaging LinkedIn posts from AI news articles, matches your personal writing style, and includes a human approval process before publishing.

![Status](https://img.shields.io/badge/status-active-success.svg)
![n8n](https://img.shields.io/badge/n8n-workflow-EA4B71?logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?logo=openai)
![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?logo=linkedin)

## ✨ Features

- **Automated News Curation** - Fetches latest AI articles from RSS feed
- **Smart Article Selection** - AI picks 5 most relevant articles for SMB executives
- **Full Content Extraction** - Downloads and parses complete article text
- **Writing Style Matching** - Replicates your unique LinkedIn voice
- **Email Approval Process** - Review and request changes before posting
- **Revision Loop** - AI applies your feedback and seeks re-approval
- **One-Click Publishing** - Posts directly to LinkedIn when approved
- **Manual Trigger** - Run on-demand when you need content

## 🔄 Workflow Overview

### Data Flow

```
Manual Trigger
     ↓
Set Writing Style
     ↓
Fetch RSS Feed (AI News)
     ↓
Extract Fields (title, link, snippet)
     ↓
Aggregate All Articles
     ↓
AI Selects Top 5 (SMB-relevant)
     ↓
Split Into Individual Articles
     ↓
Download Full Article Content
     ↓
Extract HTML Content
     ↓
Aggregate Articles
     ↓
AI Creates LinkedIn Post
     ↓
AI Matches Your Writing Style
     ↓
Email Approval Request
     ↓
  ┌─────┴─────┐
  ↓           ↓
Approved    Changes Needed
  ↓           ↓
Post to    AI Revises
LinkedIn   ↓
           Loop Back to Approval
```

## 🚀 Quick Start

### Prerequisites

- n8n instance (self-hosted or cloud)
- OpenAI API key (GPT-4o and GPT-4o-mini access)
- LinkedIn OAuth credentials
- Gmail OAuth credentials

### Setup Instructions

1. **Import Workflow**
   ```bash
   1. Download "Create_a_LinkedIn_post__with_approval_updated.json"
   2. In n8n: Import from File
   3. Upload the JSON file
   ```

2. **Configure Credentials**
   - OpenAI API key
   - LinkedIn OAuth (person ID: update in LinkedIn node)
   - Gmail OAuth

3. **Set Your Writing Style**
   - Edit the "Writing style" node
   - Paste 1-3 example LinkedIn posts in your voice
   - AI will match this style

4. **Update RSS Feed** (Optional)
   ```javascript
   // RSS Read node
   url: "https://www.artificialintelligence-news.com/feed/"
   // Change to your preferred news source
   ```

5. **Set Recipient Email**
   ```javascript
   // Gmail node
   // Uses your authenticated Gmail account
   ```

6. **Test Run**
   - Click "Test workflow" button
   - Monitor each node's execution
   - Check email for approval request

## ⚙️ Configuration

### Writing Style Customization

```javascript
// Writing style node
your_style: "Paste 1-3 of your LinkedIn posts here..."

Example:
- Use casual or professional tone
- Include emojis or keep it clean
- Long-form or concise format
- Personal anecdotes or pure insights
```

### Article Selection Criteria

```javascript
// Basic LLM Chain node
"Pick 5 articles worth reading for SMB executives.
Only pick the ones that would be interesting for 
small and medium business owners."

Modify to target:
- Enterprise executives
- Technical audiences
- Specific industries
- Different company sizes
```

### HTML Content Extraction

```javascript
// HTML node
cssSelector: ".elementor-widget-theme-post-content"

Update for different websites:
- Inspect target article page
- Find main content CSS selector
- Update in HTML extraction node
```

### LinkedIn Post Structure

```javascript
// Basic LLM Chain1 prompt
Title: "Here is everything that happened in AI this week"

Customize:
- Change title/hook
- Adjust summary length
- Modify tone instructions
- Add hashtags
- Include call-to-action
```

## 📁 Workflow Structure

```
LinkedIn Post Creator/
├── Trigger
│   └── Manual Trigger (Test workflow button)
│
├── Style Configuration
│   └── Writing style (Set node with example posts)
│
├── News Gathering
│   ├── RSS Read (AI news feed)
│   ├── Edit Fields (Extract title, link, snippet)
│   ├── Aggregate (Combine all articles)
│   └── Basic LLM Chain (Select top 5 SMB-relevant)
│
├── Content Download
│   ├── Split Out (Individual articles)
│   ├── HTTP Request (Fetch article page)
│   ├── HTML (Extract content)
│   └── Aggregate1 (Combine article contents)
│
├── Post Creation
│   ├── Basic LLM Chain1 (Create LinkedIn post)
│   └── Basic LLM Chain2 (Match writing style)
│
├── Approval Process
│   ├── Edit Fields1 (Format post)
│   ├── Gmail (Send approval email with form)
│   └── If (Check approval status)
│
├── Revision Loop (if changes needed)
│   ├── Basic LLM Chain3 (Apply modifications)
│   ├── Structured Output Parser1 (Parse modified post)
│   ├── Edit Fields2 (Format)
│   └── Loop back to Gmail approval
│
└── Publishing
    └── LinkedIn (Post to profile)
```

## 🎯 How It Works

### Phase 1: News Curation (30 seconds)

1. **RSS Feed Fetch**
   - Pulls latest articles from AI news feed
   - Extracts title, link, and snippet from each

2. **AI Article Selection**
   - GPT-4o-mini analyzes all articles
   - Selects 5 most relevant for SMB executives
   - Returns structured JSON with title and link

### Phase 2: Content Extraction (45 seconds)

3. **Article Download**
   - Makes HTTP requests to each article URL
   - Handles redirects and loads full HTML

4. **Content Parsing**
   - Extracts article text using CSS selector
   - Removes ads, navigation, and footer elements
   - Aggregates all 5 article contents

### Phase 3: Post Generation (20 seconds)

5. **Initial Draft Creation**
   - GPT-4o-mini creates LinkedIn post from articles
   - Adds engaging hook (2 sentences)
   - Summarizes each article with SMB context
   - Includes article links

6. **Style Matching**
   - GPT-4o rewrites draft to match your style
   - Maintains plain text format (no markdown)
   - Preserves your tone, structure, emoji usage

### Phase 4: Human Review (Variable)

7. **Email Approval**
   - Sends post via Gmail with approval form
   - Two dropdown options:
     - "yes" → Post immediately
     - "no - I'll add my change requests below"
   - Optional textarea for change requests

8. **Decision Point**
   - **If Approved**: Posts directly to LinkedIn
   - **If Changes Needed**: Proceeds to revision

### Phase 5: Revision (if needed)

9. **AI Modification**
   - GPT-4o applies your change requests
   - Returns modified post in JSON format
   - Loops back to approval email

10. **Re-approval**
    - Process repeats until approved
    - Unlimited revision cycles supported

### Phase 6: Publishing

11. **LinkedIn Post**
    - Uses LinkedIn API to publish
    - Posts to specified person profile
    - Post goes live immediately

## 🎨 Customization Examples

### Change Target Audience

```javascript
// Basic LLM Chain prompt
"Pick 5 articles worth reading for:"
- "CTO and technical leaders"
- "Marketing professionals"
- "SaaS founders"
- "Enterprise IT managers"
```

### Adjust Post Format

```javascript
// Basic LLM Chain1 instructions
- Add hashtags: "Include 3-5 relevant hashtags"
- Add CTA: "End with a call to action"
- Shorter: "Keep summaries under 50 words each"
- Add stats: "Include key statistics from articles"
```

### Multiple RSS Sources

```javascript
1. Add more RSS Read nodes
2. Connect all to Merge node
3. Merge outputs before Edit Fields
4. AI will select from combined sources
```

### Custom Approval Flow

```javascript
// Gmail node - modify form
- Add approval levels (manager, legal, etc.)
- Add scheduled post time option
- Add platform selection (LinkedIn + Twitter)
- Include image upload option
```

### Different HTML Selectors

```javascript
// For TechCrunch
cssSelector: ".article-content"

// For Medium
cssSelector: "article"

// For custom sites
// Inspect page → Find content div → Copy selector
```

## 🛠️ Node Breakdown

| Node Type | Count | Purpose |
|-----------|-------|---------|
| Manual Trigger | 1 | Starts workflow |
| Set | 3 | Stores/formats data |
| RSS Feed Read | 1 | Fetches news articles |
| Aggregate | 2 | Combines multiple items |
| Basic LLM Chain | 4 | AI processing steps |
| Split Out | 1 | Separates article list |
| HTTP Request | 1 | Downloads article pages |
| HTML | 1 | Extracts article content |
| Gmail | 1 | Sends approval email |
| If | 1 | Approval decision logic |
| LinkedIn | 1 | Posts to LinkedIn |
| OpenAI Chat Model | 2 | GPT-4o and GPT-4o-mini |
| Structured Output Parser | 2 | Parses JSON responses |
| Sticky Notes | 8 | Workflow documentation |

**Total Nodes**: 29

## 🔧 Troubleshooting

### No Articles Selected

```javascript
// Check RSS feed
1. Test RSS URL in browser
2. Verify feed format (RSS 2.0 or Atom)
3. Check if feed contains recent articles
4. Review AI selection criteria

// If no SMB-relevant articles found:
- Broaden selection criteria
- Try different RSS feed
- Adjust target audience description
```

### Article Content Not Extracted

```javascript
// HTML node errors
1. Check CSS selector on target website
   - Open article in browser
   - Right-click → Inspect
   - Find content container
   - Copy selector
2. Update HTML node cssSelector field
3. Test with single article first

// Common selectors:
- ".post-content"
- "article"
- ".entry-content"
- "#main-content"
```

### Style Not Matching

```javascript
// Writing style node
1. Provide 2-3 example posts (not just 1)
2. Include variety (different topics)
3. Make examples 200+ words each
4. Ensure examples match desired tone

// Basic LLM Chain2 prompt
Add more specific instructions:
- "Use short sentences like the example"
- "Include emojis sparingly"
- "Start with a question hook"
```

### Approval Email Not Received

```javascript
// Gmail node
1. Verify Gmail OAuth credentials
2. Check spam/promotions folder
3. Confirm email address is correct
4. Test with simple Gmail send first
5. Check n8n execution log for errors
```

### LinkedIn Post Fails

```javascript
// LinkedIn node
1. Verify OAuth credentials are valid
2. Check person ID is correct:
   - Go to LinkedIn profile
   - Check URL for person ID
   - Update in LinkedIn node
3. Confirm API permissions
4. Check post content length (<3000 chars)
```

### AI Returns Invalid JSON

```javascript
// Structured Output Parser
1. Check prompt clarity
2. Verify JSON schema example
3. Add error handling:
   - Enable "Continue On Fail"
   - Add fallback branch
4. Test with simpler schema first
```

### Revision Loop Doesn't Apply Changes

```javascript
// Basic LLM Chain3
1. Check change requests field is populated
2. Verify prompt references correct nodes
3. Test with explicit, clear change requests
4. Check if modified_post is properly extracted
```

## 💡 Enhancement Ideas

### Immediate Improvements
- [ ] Add image generation for post
- [ ] Include performance tracking (likes, comments)
- [ ] Support multiple LinkedIn accounts
- [ ] Add post scheduling (not immediate)
- [ ] Include analytics summary

### Advanced Features
- [ ] Cross-post to Twitter/X
- [ ] A/B test different post variations
- [ ] Sentiment analysis before posting
- [ ] Competitive content analysis
- [ ] Trending topic integration
- [ ] Automated hashtag suggestions
- [ ] Content calendar integration
- [ ] Team collaboration (multiple approvers)

### AI Enhancements
- [ ] Generate accompanying images with DALL-E
- [ ] Optimize for LinkedIn algorithm
- [ ] Personalize for follower interests
- [ ] Suggest best posting times
- [ ] Create multi-post thread automatically
- [ ] Extract key quotes for emphasis
- [ ] Add relevant statistics/data points

### Workflow Improvements
- [ ] Slack approval instead of email
- [ ] Save drafts to database
- [ ] Version control for posts
- [ ] Reuse rejected posts later
- [ ] Analytics dashboard integration
- [ ] Brand voice compliance check
- [ ] Link shortener integration

## 📊 Performance Metrics

### Execution Time
- **News gathering**: 30 seconds
- **Content extraction**: 45 seconds (5 articles)
- **Post generation**: 20 seconds
- **Human approval**: Variable (minutes to hours)
- **Publishing**: 5 seconds

**Total automated time**: ~100 seconds

### API Costs (Approximate)
- **GPT-4o-mini calls**: $0.02-0.05 per run
- **GPT-4o calls**: $0.05-0.10 per run
- **Total per post**: $0.07-0.15

### Success Factors
- Article relevance depends on RSS feed quality
- Style matching improves with better examples
- Approval rate increases with prompt refinement
- Post engagement varies by topic and timing

## 🔒 Security Best Practices

- **API Keys**: Store in n8n credentials only
- **OAuth Tokens**: Use n8n's OAuth flow
- **LinkedIn Access**: Request minimum scopes
- **Email Security**: Enable 2FA on Gmail
- **Workflow Privacy**: Don't share with credentials
- **Content Review**: Always review before posting
- **Rate Limits**: Respect LinkedIn posting limits (max 25/day)

## 📝 Credentials Setup

### OpenAI API

```
1. Visit platform.openai.com
2. Create API key
3. Add to n8n credentials
4. Enable GPT-4o and GPT-4o-mini access
```

### LinkedIn OAuth

```
1. Visit LinkedIn Developers portal
2. Create new app
3. Request API access
4. Add OAuth redirect URL (n8n webhook)
5. Get Person ID:
   - Go to your LinkedIn profile
   - Check URL: linkedin.com/in/USERNAME
   - Or use API to fetch ID
6. Add credentials to n8n
```

### Gmail OAuth

```
1. Create Google Cloud project
2. Enable Gmail API
3. Create OAuth credentials
4. Add authorized redirect URI
5. Configure in n8n
6. Authorize gmail.send scope
```

## 🌐 Deployment Options

### n8n Cloud
- Import workflow JSON
- Configure credentials
- Test manually
- Run when needed

### Self-Hosted n8n

```bash
# Docker
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

# Access: http://localhost:5678
```

### Automation Options

```javascript
// Add Schedule Trigger
- Replace Manual Trigger
- Set to weekly (e.g., every Monday 9 AM)
- Workflow runs automatically

// Add Webhook Trigger
- Create webhook URL
- Call from external service
- Integrate with content calendar
```

## 📖 Additional Resources

- [n8n Documentation](https://docs.n8n.io)
- [LinkedIn API Guide](https://docs.microsoft.com/linkedin)
- [OpenAI Best Practices](https://platform.openai.com/docs/guides/prompt-engineering)
- [RSS Feed Specification](https://validator.w3.org/feed/docs/rss2.html)

## 📄 License

This workflow is open source and available under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions welcome! Areas for improvement:

- Additional RSS sources
- Better content extraction patterns
- Enhanced approval workflows
- Multi-platform posting
- Analytics integration

1. Fork the repository
2. Create feature branch (`git checkout -b feature/MultiPlatform`)
3. Commit changes (`git commit -m 'Add Twitter posting'`)
4. Push to branch (`git push origin feature/MultiPlatform`)
5. Open Pull Request

## 👨‍💻 Author

**Redoanuzzaman**
- GitHub: [@REDOANUZZAMAN](https://github.com/REDOANUZZAMAN)
- Email: redoanuzzaman707@gmail.com
- Website: [redoan.dev](https://redoan.dev)

## 🙏 Acknowledgments

- n8n community for workflow platform
- OpenAI for GPT models
- LinkedIn API team
- RSS feed providers
- AI news content creators

## 💖 Show Your Support

Give a ⭐️ if this workflow helps your LinkedIn game!

## 🎯 Use Cases

- **Content Creators**: Regular LinkedIn presence without daily effort
- **SMB Consultants**: Share industry insights with clients
- **Tech Influencers**: Stay current on AI trends
- **Marketing Teams**: Consistent thought leadership
- **Business Coaches**: Position as industry expert
- **Recruiters**: Share relevant tech news

---

Made with 🤖 and n8n automation

**Last Updated:** October 2025
