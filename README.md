# Gram Connect

A comprehensive AI-powered multi-role e-commerce platform for local commerce connecting customers, shopkeepers, and delivery partners in rural and semi-urban areas.

## 📋 Overview

Gram Connect is a Django-based platform that enables:

- **Customers**: Discover and purchase products from local shops, manage cart, checkout, and track orders
- **Shopkeepers**: Manage products, inventory, and fulfill orders via dedicated dashboard
- **Delivery Partners**: Accept orders and manage delivery status in real-time

### Core Innovation: AI-Powered Search
- **Visual Product Search**: Find similar products using image similarity (FashionSigLIP)
- **Text Recognition**: Extract and search by text from product images (TrOCR, EasyOCR)
- **Smart Assistant**: Context-aware AI assistant for user guidance and FAQs

## ✨ Key Features

### Core Commerce
- Multi-role user authentication (customer, shopkeeper, delivery)
- Product management (add, edit, delete, categorize)
- Shopping cart and checkout per-shop
- Order lifecycle management with clear status flow
- Delivery assignment and status tracking
- Reorder previous orders with one click

### Advanced Features
- **Product Search APIs**
  - Full-text search with filters
  - Image similarity search (visual matching)
  - Text recognition from images
- **Price History & Analytics**
  - Track price changes over time
  - Trend analysis and buy recommendations
- **Location Master Data**
  - Multi-level hierarchy (state → district → mandal → village)
  - Location-based product filtering
- **Multi-Language Support**
  - English, Hindi, Tamil, Telugu, Kannada, Malayalam
  - Real-time language switching
- **AI Assistant**
  - Rule-based response engine
  - Optional model integration (local or Hugging Face API)
  - Context-aware help for all roles

## 🏗️ Project Structure

```
gram_connect/
├── members/              # Core app (user models, authentication)
│   ├── models.py        # Shopkeeper (custom user model), Order, Product, etc.
│   ├── views.py         # Web views for all roles
│   ├── search_views.py  # AI search endpoints
│   ├── ai_bot.py        # AI assistant logic
│   ├── auth_backend.py  # Custom authentication
│   └── forms.py         # Django forms
├── mysite/              # Project settings
│   ├── settings.py      # Configuration (database, auth, AI models)
│   ├── urls.py          # URL routing
│   └── wsgi.py
├── templates/           # HTML templates for all roles
├── static/              # CSS, JavaScript
├── media/               # User-uploaded images
├── locale/              # i18n translations
├── manage.py
├── requirements.txt
└── README.md
```

**Custom User Model**: `members.Shopkeeper` (configured as `AUTH_USER_MODEL`)

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- pip and virtualenv

### Basic Setup
```powershell
# 1. Activate virtual environment
./env/Scripts/Activate.ps1

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run migrations
python manage.py migrate

# 4. (Optional) Load location data
python manage.py populate_locations

# 5. Start development server
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` in your browser.

### AI Features Setup (Optional)

For full AI capabilities, follow these steps:

1. **Install AI Models**:
   ```powershell
   python install_ai_models.py
   ```

2. **Setup Hugging Face Token** (required for model downloads):
   ```powershell
   python setup_huggingface_token.py
   ```
   Or manually set:
   ```powershell
   $env:HUGGINGFACE_TOKEN = "your_token_here"
   ```

3. **Test Model Loading**:
   ```powershell
   python test_models.py
   ```

4. **Verify Configuration**:
   Visit `http://127.0.0.1:8000/api/debug/models/` to check status

## 🔌 API Endpoints

### Authentication
```
POST   /api/customer/login/
POST   /api/customer/register/
POST   /api/shopkeeper/login/
POST   /api/shopkeeper/register/
POST   /api/delivery/login/
POST   /api/delivery/register/
GET    /logout
```

### Products
```
GET    /api/products/
       ?mode=alphabetical|shopwise&q=<search_query>
       [Filtered by customer's village when logged in]
```

### AI-Powered Search
```
POST   /api/search/image/
       [Image similarity search using FashionSigLIP]
POST   /api/search/text-image/
       [Text recognition using TrOCR/EasyOCR]
GET    /api/debug/models/
       [Check model status and availability]
```

### Price History
```
GET    /api/product/<product_id>/price-history/
       [Returns min, max, avg, trend, and recommendation]
```

### Location Master Data
```
GET    /api/states/
GET    /api/districts/<state_id>/
GET    /api/mandals/<district_id>/
GET    /api/villages/<mandal_id>/
```

### Web Routes
```
GET    /                     → Home
GET    /i18n/                → Language switcher
GET    /customer/reorder/    → Reorder last order
GET    /customer/reorder/<id>/  → Reorder specific order
```

**Note**: The AI chat endpoint (`/api/ai/chat/`) is handled client-side using `members/ai_bot.py` logic without a dedicated HTTP endpoint.

## Order Status Flow

`pending → confirmed → ready → assigned → out_for_delivery → delivered` (plus `cancelled` when applicable)

## Reorder Capability

- Reorder last order: `GET /customer/reorder/`
- Reorder by order id: `GET /customer/reorder/<order_id>/`

These routes reconstruct a cart (client localStorage) and redirect to the cart/checkout flow.

## Localization (i18n)

- Enabled languages: English (`en`), Hindi (`hi`), Tamil (`ta`), Telugu (`te`), Kannada (`kn`), Malayalam (`ml`)
- Language switcher base route: `/i18n/`
- Translation files live in `locale/<lang>/LC_MESSAGES/`

## Installation & Setup (Windows PowerShell)

### Basic Setup
1. Create and activate a virtual environment (optional if `env/` exists)
   - `python -m venv env`
   - `./env/Scripts/Activate.ps1`
2. Install dependencies
   - `pip install -r requirements.txt`
3. Run database migrations
   - `python manage.py migrate`
4. (Optional) Load location master data
   - `python manage.py populate_locations`
5. Start the development server
   - `python manage.py runserver`
6. Visit `http://127.0.0.1:8000/`

### AI Features Setup (Optional)
For full AI functionality, follow these additional steps:

1. **Install AI Models** (automated script):
   ```bash
   python install_ai_models.py
   ```

2. **Setup Hugging Face Token** (for model downloads):
   ```bash
   python setup_huggingface_token.py
   ```
   Or manually set environment variable:
   ```bash
   set HUGGINGFACE_TOKEN=your_token_here
   ```

3. **Test Model Loading**:
   ```bash
   python test_models.py
   ```

4. **Verify Configuration**:
   Visit `http://127.0.0.1:8000/api/debug/models/` to check model status

### Utility Scripts
- `install_ai_models.py` - Installs and downloads AI models
- `setup_huggingface_token.py` - Interactive token setup
- `test_models.py` - Tests model loading
- `verify_models.py` - Verifies model configuration
- `set_default_categories.py` - Categorizes existing products
- `install_marqo_deps.py` - Installs additional Marqo dependencies

## Configuration

- `mysite/settings.py`
  - `AUTH_USER_MODEL = 'members.Shopkeeper'`
  - `LANGUAGES`, `LOCALE_PATHS`, `LocaleMiddleware` enabled
  - `STATIC_URL`, `MEDIA_URL`, `MEDIA_ROOT` configured

- AI Assistant (optional)
  - Local model (if `transformers` is installed): set `AI_TEXT_GEN_MODEL` (default `distilgpt2`)
  - Hugging Face Inference API: set `HF_API_TOKEN` (or `HUGGINGFACEHUB_API_TOKEN`) and optionally `HF_MODEL_ID` (default `google/gemma-2-2b-it`)

- AI Search Models (optional)
  - **Hugging Face Token**: Set `HUGGINGFACE_TOKEN` environment variable or in settings.py
  - **FashionSigLIP**: Used for image similarity search (Marqo/marqo-fashionSigLIP)
  - **TrOCR**: Used for handwritten text recognition (microsoft/trocr-base)
  - **EasyOCR**: Used for general text recognition

## AI Features

### AI-Powered Product Search
The platform includes advanced AI search capabilities:

- **Image Similarity Search**: Upload product images to find visually similar items using FashionSigLIP model
- **Text Recognition Search**: Extract text from images (handwritten or printed) using TrOCR and EasyOCR
- **Intelligent Recommendations**: AI-driven product discovery and categorization

### AI Assistant
- **Rule-based responses** for common queries (login, navigation, orders)
- **Optional model integration** with local Transformers or Hugging Face API
- **Context-aware help** for all user roles (customers, shopkeepers, delivery partners)

## Development Notes

- Products are filtered to a customer's village when a customer session exists
- Price history analysis returns min/avg/max, trend, and a human-readable recommendation
- Static assets are served from `static/`; user-uploaded images from `media/` (dev only)
- AI models are cached after first load for better performance
- Fallback mechanisms ensure search works even when AI models are unavailable

## Additional Documentation

- **[AI Search Guide](SEARCH_README.md)** - Detailed documentation for AI search features
- **[Hugging Face Setup](HUGGINGFACE_SETUP.md)** - Complete guide for setting up AI models

## License

MIT
