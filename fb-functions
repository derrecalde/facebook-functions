//Initialisation
var fAppID = 'YOUR_FB_ID';
var linkApp = 'Your_URL';

jQuery(document).ready(function() {
  jQuery.ajaxSetup({ cache: true });
  jQuery.getScript('//connect.facebook.net/fr_FR/sdk.js', function(){
    //Initialise facebook
    FB.init({
        appId      : fAppID,
        xfbml      : true,
        version    : 'v2.8'
    });
    jQuery(document).trigger("facebook:ready");
  });
});

function callBasicPermissions() {
    FB.login(function(response) {
      if( response.authResponse || !response.error ) {
        FB.api('/me?fields=id,name,email,first_name,last_name,gender,timezone,locale',
          function(responseFb) {
            //User ok
            var requestAllStatues = true;
            try {
              //Email deselected
              if(!responseFb.email || responseFb.email === undefined){
                throw new "No Email";
              }
            }
            catch(err) {
              //User refused
              requestAllStatues = false;
            }

            if( requestAllStatues === true ){
              //Traitements
              window.location.href="page.php?getId="+response.authResponse.userID+"&getEmail="+responseFb.email+"&getName="+encodeURI(responseFb.name)+"&getFirstName="+encodeURI(responseFb.first_name)+"&getLastName="+encodeURI(responseFb.last_name)+"&getGender="+responseFb.gender+"&getTimezone="+responseFb.timezone+"&getLocale="+responseFb.locale;
            } else {
              //Back to home
              window.location.href="index.php?getEmail="+responseFb.email;
            }
        });
      } else {
          //Not working or no autorisations, back home.
          window.location.href="index.php";
      }
    },{scope:'email', auth_type: 'rerequest', return_scopes: true });
      //Scope = permissions ; auth8type = redemande permissions , return_scopes = visibilite des permissions acceptees (authResponse.grantedScopes)
}

function getFriends() {
FB.login(function(response) {
  if( response.authResponse || !response.error ) {
    FB.api('/me/taggable_friends', { fields: 'id,first_name,last_name,name' },
      function(responseFb){
          var requestAllStatues = true;
          try {
            if(!responseFb.read_custom_friendlists || responseFb.read_custom_friendlists === undefined){
              throw new "No permissions";
            }
          }
          catch(err) {
            requestAllStatues = false;
          }
      });
    } else {
        window.location.href="index.php";
    }
  },{scope:'email, user_friends', auth_type: 'rerequest', return_scopes: true });
}

function getFriendsData(arrayIdUsers) {
  FB.getLoginStatus(function(response) {
    if (response.status === 'connected') {
      //the user is logged in and has authenticated your app, and response.authResponse supplies the user's ID
      var uid = response.authResponse.userID;
      var accessToken = response.authResponse.accessToken;//Get the Current User Access Token
      //Get data of users by Id
      for (var i = 0; i < arrayIdUsers.length; i++) {
        FB.api(arrayIdUsers[i]+"/?access_token="+accessToken, function(responseFb) {
          //Some functions
          //console.log(responseFb);
        });
      }

    } else if (response.status === 'not_authorized') {
      // the user is logged in to Facebook, but has not authenticated your app
    } else {
      // the user isn't logged in to Facebook.
    }
  });
}

function postOnWall(text,picture_link,link_page) {
    FB.ui({
      method: 'stream.publish',
      attachment: {
        name: 'The title',
        caption: 'My description',
        media:[
        {
          type:"image",
          src:picture_link,
          href:"http://example.com/"
          }
        ],
        description: (
          text
        ),
        href: link_page
      },
      action_links: [
       { text: 'Code', href: linkApp+link_page }
     ],
     user_prompt_message: ""
    }, function(response){});
}

function shareOnWall() {
    FB.ui({
      method: 'feed',
      name: 'My title',
      link: linkApp+'detect.php',
      caption: "My description",
    }, function(response){});

}

function inviteFriends(link_page) {
      FB.ui({
        app_id: fAppID,
        method: 'send',
        link: link_page,
        caption: "My invitation",
    }, function(response){
        //console.log(response);
    });

}

//Avaible only for app in game category
function inviteFriendsForGame() {
      FB.ui({method: 'apprequests',
      message: ''
    }, function(response){
        //console.log(response);
    });

}
