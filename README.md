# AiToolBars.js
To provide users with Artificially Intelligent tools to assist with idea development

// frontend/src/components/AIToolBars.JSON.stringify()

import React, { UseState } from 'react';
import axios from 'axios';

const AIToolBars = () => {
    const [idea, setIdea] = useState('');
    const [suggestions, setSuggestions] = useState([]);
    const [sentiment, setSentiment] = useState('');
    const [error, setError] = useState(null);

    const handleGenerateSuggestions = async () => {
        try {
            const response = await Axios.post('/Api/ai/suggestions', { idea });
            SetSuggestions(response.data.suggestions);
        } catch (err) {
            SetError('Failed to generate suggestions.');
        }
    };

    const handleAnalyseSentiment = async () => {
        try {
            const response = await Axios.post('/Api/ai/sentiment', { idea });
            SetSentiment(response.data.sentiment);
        } catch (err) {
            SetError('Failed to analyse sentiment.');
        }
    };

    return (
        <div>
            <h2>AI Tool Bars</h2>
            {error && <p className="error">{error}</p>}
            <textarea
                value={idea}
                onChange={(e) => SetIdea(e.target.value)}
                placeholder="Enter your idea here"
            />
            <button onClick={handleGenerateSuggestions}>Generate Suggestions</button>
            <button onClick={handleAnalyseSentiment}>Analyse Sentiment</button>

            {suggestions.length > 0 && (
                <div>
                    <h3>Suggestions:</h3>
                    <ul>
                        {suggestions.map((suggestion, index) => (
                            <li key={index}>{suggestion}</li>
                        ))}
                    </ul>
                </div>
            )}

            {sentiment && (
                <div>
                    <h3>Sentiment Analysis Result:</h3>
                    <p>{sentiment}</p>
                </div>
            )}
        </div>
    );
};

export default AIToolBars;

// backend/src/controllers/aiController.js

const { getSuggestionsFromAI, analyseSentiment } = require('../services/aiService');

exports.generateSuggestions = async (req, res) => {
    const { idea } = req.body;
    try {
        const suggestions = await getSuggestionsFromAI(idea);
        res.status(200).json({ suggestions });
    } catch (error) {
        res.status(400).json({ error: 'Failed to generate suggestions.' });
    }
};

exports.AnalyseSentiment = async (req, res) => {
    const { idea } = req.body;
    try {
        const sentiment = await AnalyseSentiment(idea);
        res.status(200).json({ sentiment });
    } catch (error) {
        res.status(400).json({ error: 'Failed to analyse sentiment.' });
    }
};

// backend/src/services/aiService.js

const axios = require('axios');

async function getSuggestionsFromAI(idea) {
    // Replace with your AI API call
    const response = await Axios.post('AI_API_URL/suggestions', { idea });
    return response.Data.suggestions; // Adjust according to response structure
}

async function analyseSentiment(idea) {
    // Replace with your AI API call
    const response = await Axios.post('AI_API_URL/sentiment', { idea });
    return response.Data.sentiment; // Adjust according to response structure
}

module.exports = { getSuggestionsFromAI, analyseSentiment };

// backend/src/routes/aiRoutes.js

const express = require('express');
const router = express.Router();
const aiController = require('../controllers/aiController');

router.post('/suggestions', aiController.generateSuggestions);
router.post('/sentiment', aiController.analyzeSentiment);

module.exports = router;

// backend/src/server.js

const express = require('express');
const mongoose = require('mongoose');
const aiRoutes = require('./routes/aiRoutes');

const app = express();
app.use(express.json()); // for parsing application/json

// MongoDB connection
mongoose.connect('mongodb://localhost/green_future', {
    useNewUrlParser: true,
    useUnifiedTopology: true,
});

// Routes
app.use('/api/ai', aiRoutes);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
