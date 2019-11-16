# Schema interactions meaoo - application

 

<table style="border-collapse: separate; border-spacing: 10px;">
    <thead>
        <tr>
            <th colspan="3" style="border-radius: 15px; background-color: #888;"><div align="center">Votre App</div></th>
        </tr>
        <tr valign="top">
            <th style="border-radius: 15px; background-color: #888;">
                <div style="text-align:center;">REQUETER<br />
                <i>meaoo apis endpoints</i></div>
            </th>
            <th style="border-radius: 15px; background-color: #888;">
                <div style="text-align:center;">S'ABONNER<br />
                <i>meaoo events endpoints</i></div>
            </th>
            <th style="border-radius: 15px; background-color: #888;">
                <div style="text-align:center;">COMMANDER<br />
                <i>meaoo commands endpoints</i></div>
            </th>
        </tr>
    </thead>
    <tbody>
        <tr valign="top">
            <td style="border-radius: 15px; background-color: #888;">
                AGENT​<br />
                http://agent-controller.[NAMESPACE].xp65.renault-digital.com​<br />
                <ul>
                    <li>api/user/situation/last​</li>
                </ul>
                CONTEXT​<br />
                http://context-controller.[NAMESPACE].xp65.renault-digital.com​<br />
                <ul>
                    <li>api/context/weather/current​</li>
                    <li>api/context/air/current​</li>
                </ul>
                CITY GRAPH​<br />
                http://graph.[NAMESPACE].xp65.renault-digital.com<br />
                <ul>
                    <li>processed/vehicule.json​</li>
                    <li>processed/bike.json​</li>
                    <li>processed/walk.json​</li>
                    <li>processed/subway.json​</li>
                    <li>processed/neighbours.json​</li>
                    <li>processed/subway_neighbours.json​</li>
                    <li>processed/walk_neighbours.json​</li>
                    <li>road_graph/shortest_path/bike​</li>
                    <li>road_graph/shortest_path/subway​</li>
                    <li>road_graph/shortest_path/walk​</li>
                    <li>road_graph/shortest_path/car​</li>
                    <li>subway/stations​</li>
                    <li>road_graph/traffic_conditions​</li>
                    <li>road_graph/roads_status/car​</li>
                    <li>road_graph/roads_status/bike​</li>
                    <li>road_graph/line_state​</li>
                    <li>road_graph/reset_graph/car <sup>(*)</sup>​</li>
                    <li>road_graph/reset_graph/bike <sup>(*)</sup></li>
                    <li>road_graph/reset_graph/walk <sup>(*)</sup></li>
                    <li>road_graph/reset_graph/subway <sup>(*)</sup></li>
                </ul>
                VEHICLE<br />
                http://vehicle.[NAMESPACE].xp65.renault-digital.com​<br />
                <ul>
                    <li>api/v1/vehicles</li>
                    <li>api/v1/vehicles/{id}</li>
                </ul>
                * = DEV env​
            </td>
            <td style="border-radius: 15px; background-color: #888;">
                AGENT​<br />
                Solace bus address<br />
                <ul>
                    <li>[TOPIC_PREFIX]/prod/user/situation​</li>
                    <li>[TOPIC_PREFIX]/prod/user/mission</li>
                    <li>[TOPIC_PREFIX]/prod/user/objective-reached</li>
                    <li>[TOPIC_PREFIX]/prod/user/status</li>
                </ul>
                CONTEXT​<br />
                Solace bus address<br />
                <ul>
                    <li>[TOPIC_PREFIX]/prod/context/change/weather</li>
                    <li>[TOPIC_PREFIX]/prod/context/change/air</li>
                </ul>
                CITY<br />
                Solace bus address<br />
                <ul>
                    <li>[TOPIC_PREFIX]/prod/environment/change/roads_status​</li>
                    <li>[TOPIC_PREFIX]/prod/environment/change/lines_state​</li>
                    <li>[TOPIC_PREFIX]/prod/environment/change/traffic_conditions​</li>
                    <li>[TOPIC_PREFIX]/prod/environment/change/breakdown</li>
                </ul>
                VEHICLE<br />
                Solace bus address<br />
                <ul>
                    <li>[TOPIC_PREFIX]/prod/{id}/status/attitude​</li>
                </ul>
            </td>
            <td style="border-radius: 15px; background-color: #888;">
                AGENT​<br />
                Solace bus address<br />
                <ul>
                    <li>[TOPIC_PREFIX]/prod/user/path</li>
                    <li>[TOPIC_PREFIX]/prod/user/path-to-target <sup>(*)</sup></li>
                </ul>
                CITY<br />
                Solace bus address<br />
                <ul>
                    <li>[TOPIC_PREFIX]/prod/city/reset <sup>(*)</sup></li>
                    <li>[TOPIC_PREFIX]/prod/city/morph/traffic_conditions <sup>(*)</sup></li>
                    <li>[TOPIC_PREFIX]/prod/city/morph/lines_state <sup>(*)</sup></li>
                    <li>[TOPIC_PREFIX]/prod/city/morph/roads_status <sup>(*)</sup></li>
                * = DEV env​
                </ul>
            </td>
        </tr>
    </tbody>
</table>