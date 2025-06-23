# Analytics & Performance

## Analytics Strategy

### Key Performance Indicators (KPIs)

**Primary Goals**
- Workshop inquiry conversion rate
- AI Twin engagement rate
- Time spent on workshop page
- Mobile vs desktop usage

**Secondary Metrics**
- Page load speed
- Bounce rate
- Traffic sources
- Geographic distribution
- Return visitor rate

### Analytics Implementation

**Google Analytics 4 Setup**
```html
<!-- Google Analytics 4 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  
  gtag('config', 'G-MEASUREMENT_ID', {
    page_title: 'AI Workshop Landing',
    page_location: window.location.href,
    content_group1: 'Workshop Pages'
  });
</script>
```

**Event Tracking**
```javascript
// Workshop CTA clicks
function trackWorkshopClick() {
  gtag('event', 'workshop_cta_click', {
    event_category: 'engagement',
    event_label: 'hero_section',
    value: 1
  });
}

// AI Twin link clicks
function trackAITwinClick() {
  gtag('event', 'ai_twin_click', {
    event_category: 'engagement',
    event_label: 'final_cta',
    value: 1
  });
}

// Scroll depth tracking
function trackScrollDepth(percentage) {
  gtag('event', 'scroll', {
    event_category: 'engagement',
    event_label: percentage + '%',
    value: percentage
  });
}
```

### Conversion Tracking

**Goal Setup in GA4**
```
Goal 1: Workshop Inquiry
- Event: workshop_cta_click
- Value: High priority conversion

Goal 2: AI Twin Engagement
- Event: ai_twin_click
- Value: Lead generation

Goal 3: Page Engagement
- Event: scroll depth > 75%
- Value: Content engagement
```

**Custom Dimensions**
```
Dimension 1: Device Category (Mobile/Desktop/Tablet)
Dimension 2: Traffic Source Category
Dimension 3: First Time vs Returning
Dimension 4: Geographic Region
```

## Performance Monitoring

### Core Web Vitals Tracking

**Implementation**
```javascript
// Web Vitals tracking
import { getLCP, getFID, getCLS } from 'web-vitals';

function sendToAnalytics(metric) {
  gtag('event', metric.name, {
    event_category: 'Web Vitals',
    event_label: metric.id,
    value: Math.round(metric.name === 'CLS' ? metric.value * 1000 : metric.value),
    non_interaction: true
  });
}

getLCP(sendToAnalytics);
getFID(sendToAnalytics);
getCLS(sendToAnalytics);
```

**Performance Targets**
```
Largest Contentful Paint (LCP): < 2.5s
First Input Delay (FID): < 100ms
Cumulative Layout Shift (CLS): < 0.1
First Contentful Paint (FCP): < 1.8s
Time to Interactive (TTI): < 3.8s
```

### Real User Monitoring (RUM)

**Performance Observer**
```javascript
// Monitor navigation timing
if ('PerformanceObserver' in window) {
  const observer = new PerformanceObserver((list) => {
    list.getEntries().forEach((entry) => {
      if (entry.entryType === 'navigation') {
        gtag('event', 'page_load_time', {
          event_category: 'Performance',
          event_label: 'navigation',
          value: Math.round(entry.loadEventEnd - entry.loadEventStart)
        });
      }
    });
  });
  
  observer.observe({ entryTypes: ['navigation'] });
}
```

### Lighthouse Integration

**Automated Lighthouse Checks**
```yaml
# GitHub Action for Lighthouse CI
name: Lighthouse CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Lighthouse CI
        uses: treosh/lighthouse-ci-action@v9
        with:
          urls: |
            https://valtoai.com
            https://valtoai.com/1.html
          uploadArtifacts: true
          temporaryPublicStorage: true
```

**Performance Budget**
```json
{
  "budget": [
    {
      "path": "/*",
      "timings": [
        {"metric": "first-contentful-paint", "budget": 2000},
        {"metric": "largest-contentful-paint", "budget": 2500},
        {"metric": "speed-index", "budget": 3000}
      ],
      "resourceSizes": [
        {"resourceType": "total", "budget": 500},
        {"resourceType": "image", "budget": 200},
        {"resourceType": "stylesheet", "budget": 50}
      ]
    }
  ]
}
```

## SEO Analytics

### Google Search Console Setup

**Property Configuration**
```
Property: https://valtoai.com
Verification: HTML file upload or DNS TXT record
Sitemap: https://valtoai.com/sitemap.xml (to be created)
```

**Key Metrics to Monitor**
- Search impressions
- Click-through rate (CTR)
- Average position
- Index coverage
- Core Web Vitals
- Mobile usability

### Schema Markup

**Person Schema (Valto)**
```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Valto Loikkanen",
  "jobTitle": "AI Strategist & Entrepreneur",
  "description": "Internationally awarded entrepreneur and AI strategist with 25+ years of experience",
  "url": "https://valtoai.com",
  "sameAs": [
    "https://hey.speak-to.ai/valto"
  ]
}
```

**Service Schema (AI Workshops)**
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "Private AI Workshops",
  "description": "Hands-on, custom AI workshops for business transformation",
  "provider": {
    "@type": "Person",
    "name": "Valto Loikkanen"
  },
  "serviceType": "AI Training and Consulting",
  "url": "https://valtoai.com/1.html"
}
```

## Heatmap & User Behavior

### Hotjar Integration

```html
<!-- Hotjar Tracking Code -->
<script>
    (function(h,o,t,j,a,r){
        h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
        h._hjSettings={hjid:HOTJAR_ID,hjsv:6};
        a=o.getElementsByTagName('head')[0];
        r=o.createElement('script');r.async=1;
        r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
        a.appendChild(r);
    })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
</script>
```

**Heatmap Analysis Points**
- CTA button interaction
- Scroll behavior patterns
- Mobile touch interactions
- Form field interactions
- Navigation patterns

### A/B Testing Framework

**Google Optimize Setup**
```html
<!-- Google Optimize -->
<script src="https://www.googleoptimize.com/optimize.js?id=OPT-CONTAINER_ID"></script>
```

**Test Ideas**
- Headline variations
- CTA button text/color
- Workshop benefit ordering
- About section placement
- Contact method prominence

## Privacy & Compliance

### GDPR Compliance

**Cookie Consent**
```html
<!-- Cookie Consent Banner -->
<div id="cookie-consent" class="cookie-banner">
  <p>This site uses cookies to improve your experience and analyze traffic.</p>
  <button onclick="acceptCookies()">Accept</button>
  <button onclick="declineCookies()">Decline</button>
  <a href="/privacy-policy.html">Privacy Policy</a>
</div>
```

**Data Processing**
```javascript
// Conditional analytics loading
function acceptCookies() {
  localStorage.setItem('cookies-accepted', 'true');
  loadAnalytics();
  hideCookieBanner();
}

function loadAnalytics() {
  // Load GA4, Hotjar, etc. only after consent
}
```

### Privacy Policy Requirements
- Data collection disclosure
- Cookie usage explanation
- Third-party services list
- User rights information
- Contact information for privacy inquiries

## Reporting & Dashboards

### Weekly Performance Report
```
Metrics to Track:
- Unique visitors
- Workshop CTA clicks
- AI Twin engagement
- Page load speed
- Mobile performance
- Search rankings
```

### Monthly Business Review
```
Business Metrics:
- Lead generation rate
- Conversion funnel analysis
- Traffic source performance
- Content engagement
- Technical performance
- SEO progress
```

### Dashboard Setup (Google Data Studio)
```
Data Sources:
- Google Analytics 4
- Google Search Console
- PageSpeed Insights API
- GitHub Actions (build status)

Key Visualizations:
- Traffic overview
- Conversion funnel
- Performance metrics
- SEO progress
- Mobile vs desktop usage
```

---

*Next: Review [Maintenance](./maintenance.md) for ongoing upkeep guidelines*