# Image Classification FastAPI

A FastAPI-based web service for cat and dog image classification using PyTorch and ResNet18. This project provides a RESTful API endpoint that accepts image uploads and returns classification predictions with confidence scores.

## 🚀 Features

- **Image Classification**: Classify images as either "Cat" or "Dog"
- **RESTful API**: Clean FastAPI-based web service
- **Deep Learning Model**: Uses ResNet18 backbone with custom classifier
- **Confidence Scores**: Returns probability distributions and best prediction
- **File Upload Support**: Accepts image files via multipart form data
- **Logging**: Comprehensive logging for HTTP requests and model predictions
- **CORS Support**: Cross-origin resource sharing enabled
- **Modular Architecture**: Well-organized code structure with separate modules

## 📁 Project Structure

```
Image_Classfication_FastAPI/
├── app.py                 # FastAPI application entry point
├── server.py              # Uvicorn server configuration
├── requirements.txt       # Python dependencies
├── README.md             # Project documentation
├── config/               # Configuration files
│   ├── catdog_cfg.py    # Model and data configuration
│   └── logging_cfg.py   # Logging configuration
├── middleware/           # Custom middleware
│   ├── cors.py          # CORS configuration
│   └── http.py          # HTTP logging middleware
├── models/              # ML model files
│   ├── catdog_model.py  # ResNet18-based model definition
│   ├── catdog_predictor.py # Model inference class
│   └── weights/         # Pre-trained model weights
│       └── catdog_weight.pt
├── routes/              # API route definitions
│   ├── base.py         # Main router configuration
│   └── catdog_route.py # Classification endpoint
├── schemas/             # Pydantic data models
│   └── catdog_schema.py # Response schema
├── utils/               # Utility functions
│   └── logger.py       # Logging utilities
└── logs/               # Log files
    ├── http.log        # HTTP request logs
    └── predictor.log   # Model prediction logs
```

## 🛠️ Installation

### Prerequisites

- Python 3.8+
- pip or conda

### Setup

1. **Clone the repository** (if applicable):
   ```bash
   git clone <repository-url>
   cd Image_Classfication_FastAPI
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Verify model weights**:
   Ensure the pre-trained model weights are present at:
   ```
   models/weights/catdog_weight.pt
   ```

## 🚀 Usage

### Starting the Server

Run the FastAPI server using:

```bash
python server.py
```

The server will start on `http://localhost:9000` with auto-reload enabled.

### API Documentation

Once the server is running, you can access:

- **Interactive API docs**: `http://localhost:9000/docs`
- **ReDoc documentation**: `http://localhost:9000/redoc`

### Making Predictions

#### Endpoint
```
POST /catdog_classification/predict
```

#### Request
- **Method**: POST
- **Content-Type**: multipart/form-data
- **Body**: Image file upload

#### Example using curl:
```bash
curl -X POST "http://localhost:9000/catdog_classification/predict" \
     -H "accept: application/json" \
     -H "Content-Type: multipart/form-data" \
     -F "file=@path/to/your/image.jpg"
```

#### Example using Python requests:
```python
import requests

url = "http://localhost:9000/catdog_classification/predict"
files = {"file": open("path/to/your/image.jpg", "rb")}

response = requests.post(url, files=files)
result = response.json()
print(result)
```

#### Response Format:
```json
{
  "probs": [0.123456, 0.876544],
  "best_prob": 0.876544,
  "predicted_id": 1,
  "predicted_class": "Dog",
  "predicted_name": "resnet18"
}
```

#### Response Fields:
- `probs`: List of probabilities for each class [Cat, Dog]
- `best_prob`: Highest confidence score
- `predicted_id`: Predicted class ID (0=Cat, 1=Dog)
- `predicted_class`: Predicted class name ("Cat" or "Dog")
- `predicted_name`: Model name used for prediction

## 🔧 Configuration

### Model Configuration (`config/catdog_cfg.py`)

- **Classes**: 2 (Cat, Dog)
- **Image Size**: 64x64 pixels
- **Model**: ResNet18 backbone
- **Device**: CPU (configurable)
- **Normalization**: ImageNet mean and std values

### Logging

The application logs both HTTP requests and model predictions:
- HTTP logs: `logs/http.log`
- Prediction logs: `logs/predictor.log`

## 🏗️ Architecture

### Model Architecture
- **Backbone**: ResNet18 (pre-trained on ImageNet)
- **Classifier**: Custom fully connected layer
- **Transfer Learning**: Frozen backbone, trainable classifier
- **Input**: RGB images resized to 64x64 pixels

### API Architecture
- **Framework**: FastAPI
- **Server**: Uvicorn
- **Middleware**: Custom logging and CORS
- **Validation**: Pydantic schemas
- **Async Support**: Async/await for model inference

## 📦 Dependencies

- **FastAPI**: Web framework for building APIs
- **Uvicorn**: ASGI server
- **PyTorch**: Deep learning framework
- **Torchvision**: Computer vision utilities
- **Pillow**: Image processing
- **Python-multipart**: File upload support

## 🔍 Troubleshooting

### Common Issues

1. **Model weights not found**:
   - Ensure `catdog_weight.pt` exists in `models/weights/`
   - Check file permissions

2. **CUDA out of memory**:
   - The model is configured to use CPU by default
   - Modify `DEVICE` in `config/catdog_cfg.py` if needed

3. **Image format issues**:
   - The API accepts common image formats (JPEG, PNG, etc.)
   - RGBA images are automatically converted to RGB

### Performance Notes

- Model inference runs on CPU by default
- Images are automatically resized to 64x64 pixels
- GPU memory is cleared after each prediction

## 📝 License

This project is for educational and demonstration purposes.

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📞 Support

For questions or issues, please check the logs in the `logs/` directory for detailed error information.