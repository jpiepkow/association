Association
===
Description: General machine learning helper library that exposes a nearest neighbor algorithm that can be configured to use one of two included types of mesurement. The lib includes two methods to either compare objects as a whole to see what two share the closest similarity or compare the keys from all objects to get the inverse. Supports <b>pearson correlation algo AND euclidean distance algo</b>

Example:

This example will be based off of a group of people rateing movies:

	My ratings:
	
	var jordan = {
    'Lord of the Rings':4.1,
    'The dark night':1.7,
    'Sin city':5,
    'Friday night lights':2.4
	};
	==========================

	Alec ratings:
	
	var alec = {
    'Lord of the Rings':4,
    'The dark night':1,
    'Sin city':4,
    'Friday night lights':4,
    'silence of the lambs':5,
    'dawn of the dead':6
	};
	==========================
	
	Randy ratings:
	
	var randy = {
    'Lord of the Rings':5,
    'The dark night':5,
    'Sin city':5,
    'Friday night lights':2,
    'silence of the lambs':3.9,
    'dawn of the dead':5,
    'night of the living dead':6
	};
	==========================
	
	User map:
	
	var userMap = {
    alec:alec,
    randy:randy
	}
	
With this information we can run a compareAll to see who shares my views in movies.

	var association = require('./index.js')
	var a = new association({
    	formula: 'pearson' // this can also be euclidean
	});
	
	a.compareAll(2,userMap,jordan)	
	
This will return the following(if you use pearson algo)

	[ { key: 'alec', score: 0.7033391716221717 },
  	{ key: 'randy', score: 0.3956282840374718 } ]	

  	This shows that my movie vies match up best with alec.

I can also use this information to get recommendations of movies I have not seen yet.

	var association = require('./index.js')
	var a = new association({
    	formula: 'pearson' // this can also be euclidean
	});
	
	a.getRecommendations(a.compareAll(2,userMap,jordan))
	
This will return the following:

	[ { key: 'night of the living dead', score: 6 },
  	{ key: 'dawn of the dead', score: 5.64 },
  	{ key: 'silence of the lambs', score: 4.603999999999999 } ]
  	
This shows that I will like night of the living dead the most of movies I have not viewed yet followed by dawn of the dead and then silence of the lambs.  	


Overview:

	Methods:
	
	compareAll(num,mapOfMap,baseMap);
		params:
		
			num: the number of returns in the array by the algo
			mapOfMap: the map containing all the maps that will be used 
			for the algo, example map of users movie ratings.
			baseMap: the base map that things will find relation to.
			example: my movie ratings
			
		return:			
			ranking of keys in mapOfMap by comparing objects. //correlation
			
	getReccommendations(simObj,mapOfMap,baseMap);
		params:
			
			simObj: the correlation returned by compareAll
			mapOfMap: the map containing all the maps that will be used 
			for the algo, example map of users movie ratings.
			baseMap: the base map that things will find relation to.
			example: my movie ratings
		
		return:			
			ranking of items not included in base map by similarity to existing items.
			