---
layout: page
title: Browse Transcriptions
permalink: /transcriptions/
---

<div class="transcription-filters">
  <h3>Filter by:</h3>
  
  <div class="filter-group">
    <label for="instrument-filter">Instrument:</label>
    <select id="instrument-filter">
      <option value="all">All Instruments</option>
      {% assign instruments = site.transcriptions | map: "instrument" | uniq | sort %}
      {% for instrument in instruments %}
        <option value="{{ instrument | slugify }}">{{ instrument }}</option>
      {% endfor %}
    </select>
  </div>
  
  <div class="filter-group">
    <label for="difficulty-filter">Difficulty:</label>
    <select id="difficulty-filter">
      <option value="all">All Levels</option>
      {% assign difficulties = site.transcriptions | map: "difficulty" | uniq | sort %}
      {% for difficulty in difficulties %}
        <option value="{{ difficulty | slugify }}">{{ difficulty }}</option>
      {% endfor %}
    </select>
  </div>
</div>

<ul class="transcription-list">
  {% for transcription in site.transcriptions %}
    <li class="transcription-item" 
        data-instrument="{{ transcription.instrument | slugify }}" 
        data-difficulty="{{ transcription.difficulty | slugify }}">
      <h3>
        <a href="{{ transcription.url | relative_url }}">{{ transcription.title }}</a>
      </h3>
      <div class="transcription-details">
        <span class="artist">{{ transcription.artist }}</span>
        {% if transcription.album %}<span class="album">Album: {{ transcription.album }}</span>{% endif %}
        {% if transcription.year %}<span class="year">({{ transcription.year }})</span>{% endif %}
        <span class="instrument">Instrument: {{ transcription.instrument }}</span>
        {% if transcription.difficulty %}
        <span class="difficulty {{ transcription.difficulty | slugify }}">
          {{ transcription.difficulty }}
        </span>
        {% endif %}
      </div>
    </li>
  {% endfor %}
</ul>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const instrumentFilter = document.getElementById('instrument-filter');
  const difficultyFilter = document.getElementById('difficulty-filter');
  const transcriptionItems = document.querySelectorAll('.transcription-item');
  
  function filterTranscriptions() {
    const selectedInstrument = instrumentFilter.value;
    const selectedDifficulty = difficultyFilter.value;
    
    transcriptionItems.forEach(item => {
      const instrumentMatch = selectedInstrument === 'all' || item.dataset.instrument === selectedInstrument;
      const difficultyMatch = selectedDifficulty === 'all' || item.dataset.difficulty === selectedDifficulty;
      
      if (instrumentMatch && difficultyMatch) {
        item.style.display = 'block';
      } else {
        item.style.display = 'none';
      }
    });
  }
  
  instrumentFilter.addEventListener('change', filterTranscriptions);
  difficultyFilter.addEventListener('change', filterTranscriptions);
});
</script>
