import { useState, useEffect } from 'react';
import Head from 'next/head';

export default function TradeJournal() {
  const [trades, setTrades] = useState([]);
  const [formData, setFormData] = useState({
    date: '',
    pair: 'EUR/USD',
    direction: 'Long',
    entry: '',
    exit: '',
    timeSlot: '09:00-11:00',
    outcome: 'Win',
    notes: ''
  });

  // Load saved trades from localStorage
  useEffect(() => {
    const savedTrades = localStorage.getItem('trades');
    if (savedTrades) setTrades(JSON.parse(savedTrades));
  }, []);

  // Save trades to localStorage
  useEffect(() => {
    localStorage.setItem('trades', JSON.stringify(trades));
  }, [trades]);

  const handleSubmit = (e) => {
    e.preventDefault();
    setTrades([...trades, formData]);
    setFormData({
      date: '',
      pair: 'EUR/USD',
      direction: 'Long',
      entry: '',
      exit: '',
      timeSlot: '09:00-11:00',
      outcome: 'Win',
      notes: ''
    });
  };

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  // Calculate stats
  const winRate = trades.length > 0 
    ? ((trades.filter(t => t.outcome === 'Win').length / trades.length) * 100).toFixed(2) 
    : 0;

  return (
    <div className="container">
      <Head>
        <title>Trade Journal</title>
      </Head>

      <h1>📊 Trade Journal</h1>
      
      {/* Trade Entry Form */}
      <form onSubmit={handleSubmit}>
        <input type="date" name="date" value={formData.date} onChange={handleChange} required />
        <select name="pair" value={formData.pair} onChange={handleChange}>
          <option>EUR/USD</option>
          <option>GBP/USD</option>
          <option>BTC/USD</option>
        </select>
        <select name="direction" value={formData.direction} onChange={handleChange}>
          <option>Long</option>
          <option>Short</option>
        </select>
        <input type="number" name="entry" placeholder="Entry Price" step="0.0001" value={formData.entry} onChange={handleChange} required />
        <input type="number"