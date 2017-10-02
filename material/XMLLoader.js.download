// this is XMLloader.js -- Fangyu Lin
/**
 * Description: A THREE loader for XML files
 *
 * Usage:
 *	var loader = new THREE.XMLLoader();
 *	loader.load('test.xml', function (geometry) {
 *
 *		scene.add( new THREE.Mesh( geometry ) );
 *
 *	} );
 *
 */


THREE.XMLLoader = function ( manager ) {
    
    this.manager = ( manager !== undefined ) ? manager : THREE.DefaultLoadingManager;
    
    this.propertyNameMapping = {};
    
};

THREE.XMLLoader.prototype = {
    
constructor: THREE.XMLLoader,
    
load: function ( url, onLoad, onProgress, onError ) {
    
    var scope = this;
    
    var loader = new THREE.XHRLoader( this.manager );
    loader.setResponseType( 'text' );
    loader.load( url, function ( text ) {
                
                onLoad( scope.parse( text ) );
                
                }, onProgress, onError );
    
},
    
setPropertyNameMapping: function ( mapping ) {
    
    this.propertyNameMapping = mapping;
    
},
    
parse: function ( data ) {
    
    function parseXML( data, spritey_list){
        var geometry = new THREE.Geometry();
        // var spritey_list = [];

        if (window.DOMParser) {
            parserr = new DOMParser();
            xmlDoc = parserr.parseFromString(data, "text/xml");
        } else {    // Internet Explorer
            xmlDoc = new ActiveXObject("Microsoft.XMLDOM");
            xmlDoc.async = false;
            xmlDoc.loadXML(data);
        }

        geometry.colorsNeedUpdate = true;
        var xid, xcolor, xtext, xnote, xarea, xobbox, aabb;
        var x = xmlDoc.getElementsByTagName('label');
        // var spritey_list = [];
        for (i = 0; i < x.length; i++) {
            // xid = x[i].getAttribute('id');
            xcolor = x[i].getAttribute('color');
            xtext = x[i].getAttribute('text');
            // xnote = x[i].getAttribute('note');
            // xarea = x[i].getAttribute('area');
            // xobbox = x[i].getAttribute('obbox');
            aabb = x[i].getAttribute('aabbox');
            
            if(xtext){
                aabb = aabb.split(" ");
                //console.log("Lable:", i, "; ObjName: ", xtext, "; AABBox: ", aabb); //test output
                var vertex_indx = [
                                   new THREE.Vector3(aabb[0], aabb[1], aabb[2]),  //0
                                   new THREE.Vector3(aabb[3], aabb[1], aabb[2]),  //1
                                   new THREE.Vector3(aabb[3], aabb[1], aabb[5]),  //2
                                   new THREE.Vector3(aabb[0], aabb[1], aabb[5]),  //3
                                   new THREE.Vector3(aabb[0], aabb[4], aabb[2]),  //4
                                   new THREE.Vector3(aabb[3], aabb[4], aabb[2]),  //5
                                   new THREE.Vector3(aabb[3], aabb[4], aabb[5]),  //6
                                   new THREE.Vector3(aabb[0], aabb[4], aabb[5])   //7
                                   ];
                xcolor = xcolor.split(" ");
                aCube( vertex_indx, geometry, xcolor, xtext, spritey_list);
            }
        }
        
        return geometry;
    }
    
    function aCube( inVertPts, geometry, xcolor, xtext, spritey_list)
    {
        var indices = [0,1,2,3,0,4,5,1,2,6,5,4,7,6,7,3,1,2,5,6,0,3,4,7]; //the drawline Pieces orders.
        var rgbcolor = "rgb(" + xcolor[0] + "," + xcolor[1] + "," + xcolor[2] + ")";
        // console.log(rgbcolor);   //test output

        for ( var i = 0; i < indices.length; ++i ) {
            geometry.vertices.push( inVertPts[indices[i]] );
            geometry.colors.push(new THREE.Color(rgbcolor));
        }

        // console.log("label: " + xtext);  //test output
        var atextMesh = maketextureMesh(xtext, inVertPts, xcolor);
        spritey_list.push(atextMesh);
        //console.log("here it is: ", spritey);  //test output
    }

    function maketextureMesh(xtext, inVertPts ,xcolor){
        var canvas1 = document.createElement('canvas');
        canvas1.width = 256;
        canvas1.height = 128;
        var context1 = canvas1.getContext('2d');
        context1.font = "Bold 20pt Arial";
        context1.fillStyle = "rgba(" + xcolor[0] + "," + xcolor[1] + "," + xcolor[2] + "," + 0.9 + ")";
        context1.fillText(xtext, 0, 20);
        var texture1 = new THREE.Texture(canvas1);
        texture1.needsUpdate = true;
        // console.log("label: ", xtext);  //test output
        var material1 = new THREE.MeshBasicMaterial( {map: texture1, side:THREE.DoubleSide } );
        material1.transparent = true;

        var mesh1 = new THREE.Mesh(
            new THREE.PlaneGeometry(canvas1.width/300, canvas1.height/300),
            material1
        );
        // inVertPts[6].add(inVertPts[0]);
        mesh1.position.set((Number(inVertPts[6].x)+Number(inVertPts[0].x))/2,
            (Number(inVertPts[6].y)+Number(inVertPts[0].y))/2,
            (Number(inVertPts[6].z)+Number(inVertPts[0].z))/2);
        // console.log(mesh1.position);  //test output
         //mesh1.rotation.y = Math.PI / 7.4;
         mesh1.position.y += - 0.95;
         mesh1.position.x += 0.2;
        return mesh1;
    }

    //============================================
    console.time( 'XMLLoader' );

    var property = [];
    var geometry;
    var spritey_list = [];
    // var scope = this;
    
    geometry = parseXML( data, spritey_list );
    //geometry = parseASCII( data );
    property.push(geometry);
    if(spritey_list.length > 1) {
        // console.log(spritey_list);    //test output
        property.push(spritey_list);
    }
    console.timeEnd( 'XMLLoader' );
    
    //return geometry;
    return property;
}
    
};
