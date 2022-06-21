(function(){
  var ENVIRONMENT_VAR_NAME = "triboo_ssa"; // environment variable to be created
  var SSA_VAR_NAME = "SSA_"; // storage variable name
  var CGI_URL = "//advs.brznetwork.com/cgi-bin/getsd.cgi?OUT=5"; // cgi conversion url
  
  // get a value from a storage, if defined, otherwise empty string
  var getSSAByStorage = function(storage) {
    var ssa = "";
    try {
      ssa = storage[SSA_VAR_NAME];
    } catch(err) {};
    return ssa || "";
  }
  
  // try to get an SSA value by storages.
  var getSSA = function() {
    var ssa;
    ssa = getSSAByStorage(window.localStorage);
    if (ssa == "") {
      ssa = getSSAByStorage(window.sessionStorage);
    }
    return ssa;
  }
  
  // call cgi to decode SSA
  var getJSON = function(ssa) {
    var script = document.createElement('script');
    script.onload = function() {};
    script.src = CGI_URL + "&SSA=" + ssa;
    script.type = 'text/javascript';
    script.async = 'async';
    if (document.getElementsByTagName('body').length == 0) {
      setTimeout(function(){
        getJSON(ssa);
      }, 100);
    } else {
      document.getElementsByTagName('body')[0].appendChild(script);
    }
  }
  
  // setup the environment variable
  var setEnvironmentVar = function(value) {
    var result = {};
    
    // personas
    if (value.p) {
      var personas = ["A", "B", "C", "D", "E"];
      var arr_p = value.p.split(",");
      
      var result_arr = [];
      for (var i = 0; i < arr_p.length; i++) {
        result_arr.push(personas[arr_p[i]]);
      }
      
      result.p_tda = result_arr.join(",");
    }
    
    // gender
    if (value.g && value.g != 'nd') {
      var gender_map = {
        "f": "1",
        "ff": "1",
        "fff": "1",
        "m": "0",
        "mm": "0",
        "mmm": "0"
      };
      if (gender_map[value.g]) {
        result.g_tda = gender_map[value.g];
      }
    }
    
	// facebook gender
	if (value.fg) {
		var gender_map = {
			"m": "0",
			"f": "1"
		}
		if (gender_map[value.fg]) {
			result.g_tda_fb = gender_map[value.fg];
		}
	}
	
    // age
    if (value.a && value.a > -1) {
      var ages = ["A", "B", "C", "D", "E", "F"];
      if (ages[value.a]) {
        result.f_tda = ages[value.a];
      }
    }
    
	// facebook age
	if (value.fa && value.fa != "") {
		var s = value.fa
			.replace('0', 'A')
			.replace('1', 'B')
			.replace('2', 'C')
			.replace('3', 'D')
			.replace('4', 'E')
			.replace('5', 'F');
		result.f_tda_fb = s;
	}
	
    window[ENVIRONMENT_VAR_NAME] = result;
  }
  
  window.getsd_fnc = function(value) {
    setEnvironmentVar(value);
  }
  
  getJSON(getSSA());
})();
