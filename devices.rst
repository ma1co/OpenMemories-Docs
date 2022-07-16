Supported Devices
=================

.. raw:: html

  <div id="device-table">Loading table...</div>
  <script>
  (function(ele) {
    fetch('https://raw.githubusercontent.com/ma1co/fwtool.py/master/devices.yml').then(r => r.text()).then(data => {
      var devices = {}, dev = {};
      data.split('\n').forEach(l => {
        var [k, v] = l.split(':', 2).map(v => v.trim());
        if (v && v[0] == '\'')
          v = v.substring(1, v.length - 1);
        if (l != '') {
          if (l[0] != ' ')
            devices[k] = dev = {};
          else
            dev[k] = v;
        }
      });

      var updatershellKeys = [
        'CXD4105',
        'MB8AC102',
        'CXD4115',
        'CXD4115_ilc',
        'CXD4120',
        'CXD4132',
        'CXD90014',
      ];

      var out = '<table class="docutils align-default"><thead><tr>';
      out += '<th>Model</th>';
      out += '<th>Architecture</th>';
      out += '<th>App Support</th>';
      out += '<th>Updatershell Support</th>';
      out += '<th>Serviceshell Support</th>';
      out += '</tr></thead><tbody>';
      Object.entries(devices).forEach(([k, v]) => {
        var key = v['arch'] + (v['key'] ? ('_' + v['key']) : '');
        out += '<tr>';
        out += '<td>' + k + '</td>';
        out += '<td>' + v['arch'] + '</td>';
        out += '<td>' + (v['apps'] ? ('&#10004; (' + v['apps'] + ')') : '&#10006;') + '</td>';
        out += '<td>' + ((v['model'] && updatershellKeys.indexOf(key) >= 0) ? '&#10004;' : '&#10006;') + '</td>';
        out += '<td>&#10004;</td>';
        out += '</tr>';
      });
      out += '</tbody></table>';

      ele.innerHTML = out;
    });
  })(document.getElementById('device-table'));
  </script>
