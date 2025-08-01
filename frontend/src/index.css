import React, { useState, useEffect } from 'react';
import {
  AppBar, Toolbar, Typography, Button, Container, Box,
  TextField, List, ListItem, ListItemText, Paper, Grid,
  Alert, IconButton, Snackbar, Card, CardContent
} from '@mui/material';
import CloseIcon from '@mui/icons-material/Close';
import { BrowserRouter, Routes, Route, useNavigate } from 'react-router-dom';

// This is a placeholder for your actual Log function.
// IMPORTANT: You must replace this with the Log function you created during the pre-test.
// This function should make an API call to the logging service.
const Log = (stack, level, pkg, message) => {
  console.log(`[LOG] Stack: ${stack}, Level: ${level}, Package: ${pkg}, Message: ${message}`);
  // Your actual Log function will perform a fetch request here.
  // Example:
  // fetch('http://28.244.56.144/evaluation-service/logs', {
  //   method: 'POST',
  //   headers: {
  //     'Content-Type': 'application/json',
  //     'Authorization': `Bearer YOUR_ACCESS_TOKEN`
  //   },
  //   body: JSON.stringify({ stack, level, package: pkg, message })
  // });
};

// Simple helper function to generate a unique shortcode
const generateShortcode = () => {
  return Math.random().toString(36).substring(2, 8);
};

// --- Page Components ---

// URL Shortener Page
const UrlShortenerPage = ({ shortenedUrls, setShortenedUrls }) => {
  const [urls, setUrls] = useState(['']);
  const [shortcodes, setShortcodes] = useState(['']);
  const [validities, setValidities] = useState(['']);
  const [validationErrors, setValidationErrors] = useState([]);
  const [snackbar, setSnackbar] = useState({ open: false, message: '', severity: 'success' });
  const navigate = useNavigate();

  // Update state when a text field changes
  const handleUrlChange = (index, event) => {
    const newUrls = [...urls];
    newUrls[index] = event.target.value;
    setUrls(newUrls);
  };

  const handleShortcodeChange = (index, event) => {
    const newShortcodes = [...shortcodes];
    newShortcodes[index] = event.target.value;
    setShortcodes(newShortcodes);
  };

  const handleValidityChange = (index, event) => {
    const newValidities = [...validities];
    newValidities[index] = event.target.value;
    setValidities(newValidities);
  };

  // Add a new URL input field
  const addUrlInput = () => {
    if (urls.length < 5) {
      setUrls([...urls, '']);
      setShortcodes([...shortcodes, '']);
      setValidities([...validities, '']);
    } else {
      setSnackbar({ open: true, message: 'You can only shorten up to 5 URLs at a time.', severity: 'warning' });
      Log('frontend', 'warn', 'component', 'Attempted to add more than 5 URLs for shortening');
    }
  };

  const removeUrlInput = (index) => {
    const newUrls = urls.filter((_, i) => i !== index);
    const newShortcodes = shortcodes.filter((_, i) => i !== index);
    const newValidities = validities.filter((_, i) => i !== index);
    setUrls(newUrls);
    setShortcodes(newShortcodes);
    setValidities(newValidities);
  };

  // Client-side validation function
  const validateInputs = () => {
    const errors = [];
    const usedShortcodes = new Set();
    urls.forEach((url, index) => {
      // Validate URL format
      if (!url.startsWith('http://') && !url.startsWith('https://')) {
        errors.push(`URL at row ${index + 1} is malformed.`);
        Log('frontend', 'error', 'component', `Validation failed for URL at row ${index + 1}: Malformed URL.`);
      }
      // Validate validity period
      const validity = validities[index];
      if (validity && (!Number.isInteger(Number(validity)) || Number(validity) <= 0)) {
        errors.push(`Validity period at row ${index + 1} must be a positive integer.`);
        Log('frontend', 'error', 'component', `Validation failed for URL at row ${index + 1}: Invalid validity period.`);
      }
      // Validate shortcode uniqueness
      const shortcode = shortcodes[index];
      if (shortcode) {
        if (usedShortcodes.has(shortcode) || shortenedUrls.some(item => item.shortcode === shortcode)) {
          errors.push(`Shortcode "${shortcode}" at row ${index + 1} is not unique.`);
          Log('frontend', 'error', 'component', `Validation failed for URL at row ${index + 1}: Duplicate shortcode.`);
        }
        usedShortcodes.add(shortcode);
      }
    });
    setValidationErrors(errors);
    return errors.length === 0;
  };

  // Handle form submission
  const handleSubmit = (event) => {
    event.preventDefault();
    if (!validateInputs()) {
      Log('frontend', 'error', 'component', 'Client-side validation failed.');
      return;
    }

    const newShortenedUrls = urls.map((url, index) => {
      const shortcode = shortcodes[index] || generateShortcode();
      const validity = validities[index] ? parseInt(validities[index]) : 30;
      const expiryDate = new Date(new Date().getTime() + validity * 60000);

      // Simulate API call to the backend
      Log('frontend', 'info', 'api', `Submitting URL for shortening: ${url}`);
      return { originalUrl: url, shortcode, expiryDate, createdAt: new Date() };
    });

    // Update the main state of the application
    setShortenedUrls([...shortenedUrls, ...newShortenedUrls]);
    setUrls(['']);
    setShortcodes(['']);
    setValidities(['']);
    setSnackbar({ open: true, message: 'URLs shortened successfully!', severity: 'success' });
    Log('frontend', 'info', 'component', 'URLs processed and shortened successfully.');
  };

  return (
    <Container maxWidth="md" sx={{ mt: 4 }}>
      <Typography variant="h4" component="h1" gutterBottom sx={{ textAlign: 'center' }}>
        URL Shortener
      </Typography>
      <Paper elevation={3} sx={{ p: 4, mb: 4, borderRadius: 2 }}>
        <Typography variant="body1" align="center" sx={{ mb: 2 }}>
          Welcome to the Affordmed URL Shortener service. This application allows you to
          create custom, short links for up to 5 URLs at a time. You can set optional
          custom shortcodes and validity periods for each link. All link data is
          securely managed on the client-side for this evaluation.
        </Typography>
      </Paper>
      <Paper elevation={3} sx={{ p: 4, borderRadius: 2 }}>
        <form onSubmit={handleSubmit}>
          <Grid container spacing={3}>
            {urls.map((url, index) => (
              <React.Fragment key={index}>
                <Grid item xs={12} sm={4}>
                  <TextField
                    fullWidth
                    label="Original URL"
                    variant="outlined"
                    value={url}
                    onChange={(e) => handleUrlChange(index, e)}
                    required
                  />
                </Grid>
                <Grid item xs={12} sm={4}>
                  <TextField
                    fullWidth
                    label="Custom Shortcode (Optional)"
                    variant="outlined"
                    value={shortcodes[index]}
                    onChange={(e) => handleShortcodeChange(index, e)}
                  />
                </Grid>
                <Grid item xs={12} sm={4}>
                  <TextField
                    fullWidth
                    label="Validity (in minutes, default 30)"
                    variant="outlined"
                    type="number"
                    value={validities[index]}
                    onChange={(e) => handleValidityChange(index, e)}
                  />
                </Grid>
                {urls.length > 1 && (
                    <Grid item xs={12}>
                        <Button onClick={() => removeUrlInput(index)} color="error" variant="outlined">Remove</Button>
                    </Grid>
                )}
              </React.Fragment>
            ))}
          </Grid>
          <Box sx={{ mt: 3, display: 'flex', gap: 2, alignItems: 'center' }}>
            <Button
              variant="contained"
              color="primary"
              onClick={addUrlInput}
              disabled={urls.length >= 5}
            >
              Add another URL
            </Button>
            <Button
              type="submit"
              variant="contained"
              color="secondary"
            >
              Shorten
            </Button>
          </Box>
        </form>
      </Paper>
      {validationErrors.length > 0 && (
        <Alert severity="error" sx={{ mt: 3, borderRadius: 2 }}>
          <Typography variant="h6">Validation Errors:</Typography>
          <List dense>
            {validationErrors.map((error, index) => (
              <ListItem key={index}>
                <ListItemText primary={`- ${error}`} />
              </ListItem>
            ))}
          </List>
        </Alert>
      )}
      <Snackbar
        open={snackbar.open}
        autoHideDuration={6000}
        onClose={() => setSnackbar({ ...snackbar, open: false })}
        anchorOrigin={{ vertical: 'bottom', horizontal: 'center' }}
      >
        <Alert onClose={() => setSnackbar({ ...snackbar, open: false })} severity={snackbar.severity} sx={{ width: '100%' }}>
          {snackbar.message}
        </Alert>
      </Snackbar>
    </Container>
  );
};

// Shortened Links Page
const ShortenedLinksPage = ({ shortenedUrls }) => {
  return (
    <Container maxWidth="md" sx={{ mt: 4 }}>
      <Typography variant="h4" component="h1" gutterBottom sx={{ textAlign: 'center' }}>
        Shortened Links
      </Typography>
      <Paper elevation={3} sx={{ p: 4, borderRadius: 2 }}>
        {shortenedUrls.length === 0 ? (
          <Typography variant="body1" align="center">
            No URLs have been shortened yet.
          </Typography>
        ) : (
          <List>
            {shortenedUrls.map((item, index) => (
              <Card key={index} sx={{ mb: 2, borderRadius: 2, backgroundColor: '#f0f0f0' }}>
                <CardContent>
                  <Typography variant="body1" color="text.secondary">
                    Original URL: {item.originalUrl}
                  </Typography>
                  <Typography variant="h6" component="div">
                    Shortened URL: <a href={item.originalUrl} target="_blank" rel="noopener noreferrer">{window.location.origin}/{item.shortcode}</a>
                  </Typography>
                  <Typography variant="body2" color="text.secondary">
                    Expires: {item.expiryDate.toLocaleString()}
                  </Typography>
                </CardContent>
              </Card>
            ))}
          </List>
        )}
      </Paper>
    </Container>
  );
};

// Statistics Page
const UrlStatsPage = ({ shortenedUrls }) => {
  const totalUrls = shortenedUrls.length;
  const expiredUrls = shortenedUrls.filter(item => new Date() > item.expiryDate).length;
  const activeUrls = totalUrls - expiredUrls;
  const averageValidity = totalUrls > 0 ? ((shortenedUrls.reduce((sum, item) => sum + (item.expiryDate.getTime() - item.createdAt.getTime()), 0) / totalUrls) / 60000).toFixed(2) : 0;

  return (
    <Container maxWidth="md" sx={{ mt: 4 }}>
      <Typography variant="h4" component="h1" gutterBottom sx={{ textAlign: 'center' }}>
        Application Statistics
      </Typography>
      <Grid container spacing={3} sx={{ mt: 2 }}>
        <Grid item xs={12} sm={4}>
          <Paper elevation={3} sx={{ p: 3, textAlign: 'center', borderRadius: 2 }}>
            <Typography variant="h6">Total Links</Typography>
            <Typography variant="h4">{totalUrls}</Typography>
          </Paper>
        </Grid>
        <Grid item xs={12} sm={4}>
          <Paper elevation={3} sx={{ p: 3, textAlign: 'center', borderRadius: 2 }}>
            <Typography variant="h6">Active Links</Typography>
            <Typography variant="h4">{activeUrls}</Typography>
          </Paper>
        </Grid>
        <Grid item xs={12} sm={4}>
          <Paper elevation={3} sx={{ p: 3, textAlign: 'center', borderRadius: 2 }}>
            <Typography variant="h6">Expired Links</Typography>
            <Typography variant="h4">{expiredUrls}</Typography>
          </Paper>
        </Grid>
        <Grid item xs={12}>
          <Paper elevation={3} sx={{ p: 3, textAlign: 'center', borderRadius: 2 }}>
            <Typography variant="h6">Average Validity</Typography>
            <Typography variant="h4">{averageValidity} min</Typography>
          </Paper>
        </Grid>
      </Grid>
    </Container>
  );
};

// Redirection Component to handle shortcode URLs
const Redirector = ({ shortenedUrls }) => {
  const navigate = useNavigate();
  const path = window.location.pathname.substring(1);

  useEffect(() => {
    if (path) {
      const urlItem = shortenedUrls.find(item => item.shortcode === path);
      if (urlItem && new Date() < urlItem.expiryDate) {
        window.location.replace(urlItem.originalUrl);
        Log('frontend', 'info', 'route', `Redirecting from shortcode "${path}" to "${urlItem.originalUrl}".`);
      } else {
        Log('frontend', 'warn', 'route', `Invalid or expired shortcode: "${path}".`);
      }
    }
  }, [path, shortenedUrls]);

  return (
    <Container maxWidth="sm" sx={{ mt: 4 }}>
      <Alert severity="error" variant="filled">
        <Typography variant="h6">Invalid Shortcode</Typography>
        <Typography>The requested URL could not be found or has expired.</Typography>
      </Alert>
    </Container>
  );
};

// Main App Component with Router
const App = () => {
  const [shortenedUrls, setShortenedUrls] = useState([]);
  const [currentQuote, setCurrentQuote] = useState('');
  const [currentTime, setCurrentTime] = useState(new Date());

  // List of quotes related to medicine, technology, and innovation
  const quotes = [
    "Technology and medicine are inseparably linked in the future of healthcare.",
    "Innovation is the key to solving the world's most pressing medical challenges.",
    "Affordability in healthcare is not just a dream, but a necessity we must build.",
    "The art of medicine consists of amusing the patient while nature cures the disease.",
    "The future of medicine is here, and it's built on code and compassion.",
    "With every line of code, we bring a healthier future closer to reality."
  ];

  // Set up a timer to change the quote every 10 seconds
  useEffect(() => {
    // Set an initial random quote
    setCurrentQuote(quotes[Math.floor(Math.random() * quotes.length)]);

    const timer = setInterval(() => {
      setCurrentQuote(quotes[Math.floor(Math.random() * quotes.length)]);
    }, 10000); // Change quote every 10 seconds

    // Clean up the timer when the component unmounts
    return () => clearInterval(timer);
  }, []);

  // Set up a timer to update the current time
  useEffect(() => {
    const timer = setInterval(() => {
      setCurrentTime(new Date());
    }, 1000);
    // Clean up the timer when the component unmounts
    return () => clearInterval(timer);
  }, []);


  return (
    <BrowserRouter>
      <Box sx={{
        flexGrow: 1,
        minHeight: '100vh',
        backgroundImage: 'url(https://images.unsplash.com/photo-1579621970795-87facc2f976d?q=80&w=2000)',
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        backgroundAttachment: 'fixed',
        display: 'flex',
        flexDirection: 'column',
      }}>
        <AppBar position="static">
          <Toolbar>
            <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
              Affordmed URL Shortener
            </Typography>
            <Typography variant="body1" sx={{ mr: 2 }}>
              {currentTime.toLocaleTimeString()}
            </Typography>
            <Button color="inherit" component="a" href="/">Shorten URL</Button>
            <Button color="inherit" component="a" href="/links">Shortened Links</Button>
            <Button color="inherit" component="a" href="/stats">Statistics</Button>
          </Toolbar>
        </AppBar>
        <Box sx={{
          flexGrow: 1,
          display: 'flex',
          flexDirection: 'column',
          alignItems: 'center',
          justifyContent: 'center',
          textAlign: 'center',
          p: 2,
          '&::before': {
            content: '""',
            position: 'absolute',
            top: 0,
            left: 0,
            width: '100%',
            height: '100%',
            backgroundColor: 'rgba(255, 255, 255, 0.8)',
            zIndex: -1,
          },
        }}>
          <Typography variant="h5" component="h2" sx={{ mb: 4, fontStyle: 'italic' }}>
            "{currentQuote}"
          </Typography>
          <Routes>
            <Route path="/" element={
              <UrlShortenerPage
                shortenedUrls={shortenedUrls}
                setShortenedUrls={setShortenedUrls}
              />
            } />
            <Route path="/links" element={
              <ShortenedLinksPage shortenedUrls={shortenedUrls} />
            } />
            <Route path="/stats" element={
              <UrlStatsPage shortenedUrls={shortenedUrls} />
            } />
            <Route path="/:shortcode" element={
              <Redirector shortenedUrls={shortenedUrls} />
            } />
          </Routes>
        </Box>
      </Box>
    </BrowserRouter>
  );
};

export default App;
