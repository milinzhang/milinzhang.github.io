---
layout: page
title: Conference Deadlines
---

Submission deadlines for conferences I regularly follow.

<style>
#tag-filters { margin-bottom: 16px; }
#tag-filters button {
  margin-right: 6px;
  margin-bottom: 8px;
  border-radius: 20px;
  padding: 5px 16px;
  background: #fff;
  transition: background 0.15s, color 0.15s;
}
#tag-filters button[data-tag="all"] { color: #555; border-color: #bbb; }
#tag-filters button[data-tag="all"].active { background: #555; border-color: #555; color: #fff; }
#tag-filters button[data-tag="AIML"] { color: #4a7bd4; border-color: #4a7bd4; }
#tag-filters button[data-tag="AIML"].active { background: #4a7bd4; color: #fff; }
#tag-filters button[data-tag="Wireless"] { color: #3fa66b; border-color: #3fa66b; }
#tag-filters button[data-tag="Wireless"].active { background: #3fa66b; color: #fff; }
#tag-filters button[data-tag="Security"] { color: #d4664a; border-color: #d4664a; }
#tag-filters button[data-tag="Security"].active { background: #d4664a; color: #fff; }

#deadlines-table { border-collapse: separate; border-spacing: 0; }
#deadlines-table th {
  font-size: 80%;
  text-transform: uppercase;
  letter-spacing: 0.03em;
  color: #888;
  font-weight: 600;
  border-bottom: 2px solid #ddd;
}
#deadlines-table td, #deadlines-table th { padding: 14px 16px; vertical-align: middle; }
#deadlines-table tbody tr { border-bottom: 1px solid #eee; }
#deadlines-table tbody tr:hover { background: #fafafa; }

.conf-name { font-weight: 600; }
.tag-badge {
  display: inline-block;
  margin-left: 6px;
  padding: 2px 8px;
  border-radius: 10px;
  font-size: 68%;
  font-weight: 600;
  color: #fff;
  vertical-align: middle;
}
.tag-AIML { background: #4a7bd4; }
.tag-Wireless { background: #3fa66b; }
.tag-Security { background: #d4664a; }

.venue-dates { display: block; font-size: 85%; color: #999; margin-top: 2px; }
.deadline-row { white-space: nowrap; }
.deadline-row + .deadline-row { margin-top: 3px; }
.deadline-label { display: inline-block; width: 32px; font-size: 78%; color: #999; }
</style>

{% assign tag_defs = "AIML,Wireless,Security" | split: "," %}
<div class="btn-group" id="tag-filters" role="group" aria-label="Filter by area">
  <button type="button" class="btn btn-default active" data-tag="all">All</button>
  {% for t in tag_defs %}{% assign parts = t | split: ":" %}<button type="button" class="btn btn-default" data-tag="{{ parts[0] }}" title="{{ parts[1] }}">{{ parts[0] }}</button>
  {% endfor %}
</div>

<table class="table" id="deadlines-table">
  <thead>
    <tr>
      <th>Conference</th>
      <th>Deadlines</th>
      <th>Venue</th>
    </tr>
  </thead>
  <tbody>
    {% for c in site.data.deadlines %}
    <tr data-reg-deadline="{{ c.registration_deadline_utc }}" data-sub-deadline="{{ c.submission_deadline_utc }}" data-tags="{{ c.tags | join: ',' }}" data-location="{{ c.location }}">
      <td>
        <a class="conf-name" href="{{ c.link }}" target="_blank">{{ c.name }}</a>
        {% for t in c.tags %}<span class="label tag-badge tag-{{ t }}">{{ t }}</span>{% endfor %}
      </td>
      <td>
        {% if c.registration_display and c.registration_display != "" %}<div class="deadline-row"><span class="deadline-label">Reg</span>{{ c.registration_display }}</div>{% endif %}
        {% if c.submission_display and c.submission_display != "" %}<div class="deadline-row"><span class="deadline-label">Sub</span>{{ c.submission_display }}</div>{% endif %}
      </td>
      <td>
        {% if c.location and c.location != "" %}{{ c.location }}{% endif %}
        {% if c.conf_dates and c.conf_dates != "" %}<small class="venue-dates">{{ c.conf_dates }}</small>{% endif %}
      </td>
    </tr>
    {% endfor %}
  </tbody>
</table>

<script>
(function () {
  var table = document.getElementById('deadlines-table');
  var tbody = table.querySelector('tbody');
  var rows = Array.prototype.slice.call(tbody.querySelectorAll('tr'));

  // A row can have a registration deadline, a submission deadline, both, or neither.
  // "target" picks the one that matters right now: the soonest one still in the
  // future, or (if both have passed) the later of the two, for grouping/sort purposes.
  function getCandidates(row) {
    var reg = row.getAttribute('data-reg-deadline');
    var sub = row.getAttribute('data-sub-deadline');
    var candidates = [];
    if (reg) { candidates.push(new Date(reg)); }
    if (sub) { candidates.push(new Date(sub)); }
    return candidates;
  }

  function targetDeadline(row, now) {
    var candidates = getCandidates(row);
    if (!candidates.length) { return null; }
    var future = candidates.filter(function (d) { return d.getTime() > now; });
    if (future.length) {
      return future.reduce(function (a, b) { return a < b ? a : b; });
    }
    return candidates.reduce(function (a, b) { return a > b ? a : b; });
  }

  // Split into upcoming (+ TBA) vs. past, and add a collapsible "past" section.
  // Rows whose nearest deadline is within 14/30 days get a highlight.
  var pastTbody = null;
  (function sortAndSplit() {
    var now = Date.now();
    var upcoming = [], past = [], tba = [];
    rows.forEach(function (row) {
      var candidates = getCandidates(row);
      if (!candidates.length) { tba.push(row); return; }
      var target = targetDeadline(row, now);
      var diff = target.getTime() - now;
      if (diff < 0) { past.push(row); }
      else {
        upcoming.push(row);
        row.classList.toggle('danger', diff < 14 * 86400000);
        row.classList.toggle('warning', diff >= 14 * 86400000 && diff < 30 * 86400000);
      }
    });
    upcoming.sort(function (a, b) { return targetDeadline(a, now) - targetDeadline(b, now); });
    past.sort(function (a, b) { return targetDeadline(b, now) - targetDeadline(a, now); });

    tbody.innerHTML = '';
    upcoming.concat(tba).forEach(function (row) { tbody.appendChild(row); });

    if (past.length) {
      var toggleRow = document.createElement('tr');
      var toggleCell = document.createElement('td');
      toggleCell.colSpan = 3;
      var btn = document.createElement('button');
      btn.className = 'btn btn-link btn-xs';
      function label(hidden) {
        return (hidden ? 'Show ' : 'Hide ') + past.length + ' past deadline' + (past.length === 1 ? '' : 's') + (hidden ? ' ▾' : ' ▴');
      }
      btn.textContent = label(true);
      btn.addEventListener('click', function () {
        var hidden = pastTbody.style.display === 'none';
        pastTbody.style.display = hidden ? '' : 'none';
        btn.textContent = label(!hidden);
      });
      toggleCell.appendChild(btn);
      toggleRow.appendChild(toggleCell);
      table.appendChild(toggleRow);

      pastTbody = document.createElement('tbody');
      pastTbody.id = 'past-rows';
      pastTbody.style.display = 'none';
      past.forEach(function (row) { row.classList.add('text-muted'); pastTbody.appendChild(row); });
      table.appendChild(pastTbody);
    }
  })();

  // Tag filters (OR logic across the selected area).
  var filterButtons = Array.prototype.slice.call(document.querySelectorAll('#tag-filters button'));
  var active = 'all';
  function applyFilter() {
    table.querySelectorAll('tr[data-tags]').forEach(function (row) {
      var tags = (row.getAttribute('data-tags') || '').split(',');
      row.style.display = (active === 'all' || tags.indexOf(active) !== -1) ? '' : 'none';
    });
  }
  filterButtons.forEach(function (btn) {
    btn.addEventListener('click', function () {
      active = btn.getAttribute('data-tag');
      filterButtons.forEach(function (b) { b.classList.toggle('active', b === btn); });
      applyFilter();
    });
  });
})();
</script>
