---
layout: post
title:  "Code highlighting"
date:   2015-10-21 18:51:23
categories: blog update
comments: true
---
Some codesnippets from languages I use<!--break-->.

Html
{% highlight html %}
<table class="table table-condensed table-striped table-hover">
    <thead>
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Conference</th>
        <th>Stadium</th>
        <th>City</th>
        <th>State</th>
        <th>CC</th>
        <th></th>
    </tr>
    </thead>
    <tfoot>
        <tr>
            <th class="right">Total</th>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
        </tr>
    </tfoot>
    <tbody>
        <tr data-ng-repeat="c in vm.teams" class="team-list">
            <td>{{c.name | limitTo: 2000 }}</td>
            <td>{{c.type }}</td>
            <td>{{c.conference }}</td>
            <td>{{c.stadium }}</td>
            <td>{{c.city }}</td>
            <td>{{c.state }}</td>
            <td>{{c.countryCode }}</td>
            <!--<td><a ui-sref="dashboard.nfl_team({teamName: c.name})">Edit</a></td>-->
            <!--<td><button ng-click="vm.deleteNflTeam(c.name)">Delete</button></td>-->

            <!--<td>-->
                <!--<div class="btn-toolbar">-->
                    <!--<div class="btn-group">-->
                        <!--<i class="btn icon-save" ng-click="updatePerson(person)">x</i>-->
                        <!--<i class="btn icon-remove" ng-click="vm.deleteNflTeam(c.name)">x</i>-->
                    <!--</div>-->
                <!--</div>-->
            <!--</td>-->
            <td>
                <div class="btn-toolbar">
                    <div class="btn-group btn-sm">
                        <button type="button" class="btn btn-default btn-xs" ui-sref="dashboard.nfl_team({teamName: c.name})">Edit</button>
                        <button type="button" class="btn btn-default btn-xs"
                                ng-click="vm.deleteNflTeam(c.name)"
                                popover="Click to delete this team"
                                popover-trigger="mouseenter">Delete</button>
                    </div>
                </div>
            </td>
        </tr>
    </tbody>
</table>

<button class="btn btn-danger btn-outline" ui-sref="dashboard.nfl_team">Add</button>
{% endhighlight %}

CSS
{% highlight css %}
@-webkit-keyframes webkit-spin {
  from {
    -webkit-transform: scale(1) rotate(0deg);
  }
  to {
    -webkit-transform: scale(1) rotate(359deg);
  }
}
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.loading-icon {
  -ms-animation: spin .7s infinite linear;
  -moz-animation: spin .7s infinite linear;
  -webkit-animation: webkit-spin .7s infinite linear;
  /*top: 2px;
  margin-left: 7px;*/
}
{% endhighlight %}

Javascript
{% highlight js %}
(function () {
    'use strict';

    angular
        .module('app.cities')
        .factory('cityService', cityService);

    /* @ngInject */
    function cityService($http, API_PATH) {

        var apiServer = API_PATH;
        var endPoint = '/api/cities/';

        return {
            getCities: getCities
        };

        function getCities() {
            return $http.get(apiServer + endPoint, {cache: true})
                .then(getCitiesComplete)
                .catch(function (message, status) {
                    //exception.catcher('XHR Failed for getNflTeams')(message);
                });

            function getCitiesComplete(data, status, headers) {
                return data.data;
            }
        }

    }
})();
{% endhighlight %}

C#
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.IO;
using Nancy;
using Nancy.Json;
using Newtonsoft.Json;
using Owin.StatelessAuth;

namespace ePis.Api.Modules.Cities
{
    public class CityModule : NancyModule
    {
        public CityModule(IConfigProvider configProvider, IJwtWrapper jwtWrapper) : base("/api/cities")
        {
            Get["/"] = x =>
            {
                JsonSettings.MaxJsonLength = Int32.MaxValue;
                var file = Directory.GetCurrentDirectory() + @"\data\Cities.json";
                List<City> cities;
                using (var r = new StreamReader(file))
                {
                    string json = r.ReadToEnd();
                    cities = JsonConvert.DeserializeObject<List<City>>(json);
                }

                //var citiesx = cities.Take(10);
                return Negotiate.WithModel(cities)
                    .WithMediaRangeModel("application/json", cities);
            };

        }
    }
}
{% endhighlight %}
