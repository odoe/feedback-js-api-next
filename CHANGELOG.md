# Change log

The `next` version of 4.17 is now available.  Planned release date is October 2020.

![Current build version](https://img.shields.io/npm/v/arcgis-js-api/next?label=Current%20build)

## ES modules

Consuming the API with [@arcgis/core ES modules](https://www.npmjs.com/package/@arcgis/core) will be an additional option alongside the existing AMD modules. We are looking for your help to ensure that the new modules work seamlessly with modern web developer tooling.

As opposed to most `next` features, this is NOT planned for release at 4.17.

## StreamLayer updates

### Performance and stability updates

The StreamLayer can now handle faster websocket streams. When a client encounters a stream that is emitting events faster than it can handle, the API will down-throttle service updates accordingly. This along with other changes under the hood should improve overall stability, and allow clients to work with much faster stream services than they could in the past.

We added an update-rate event on the StreamLayerView, which can be used to check the websocket and client update rates. You can also can take advantage of maxReconnectionAttempts and maxReconnectionInterval properties on the StreamLayer. These properties can be used to specify how many times, and how long, to wait between attempts when the StreamLayer tries to reconnect to the server in case of lost connections.

### Consuming custom stream services

The StreamLayer can now consume custom stream services that are pushed via websocket. To do this, set the webSocketUrl property of the StreamLayer to point to your custom service. Please check out this repo for creating custom stream services to get started.

## VectorTileLayer updates

At 4.17, VectorTileLayer has been updated in many areas. Here are updates that will visually affect the VectorTileLayer for the better. Labels will no longer overlap near tile boundaries. You will also notice that labels will not be cut-off near tile boundaries while you are zooming in and out. We also fade symbols in and out when you interact with map instead of abruptly turning visibility of symbols on and off.

## Client-side layer updates

CSVLayer, GeoJSONLayer, and FeatureLayer created from client-side graphics are now taking advantage of a worker to store the data. This means you can perform more queries on your client-side layers in a background thread without interfering with the user interface.

## Raster updates

* At this release, we are introducing VectorFieldRenderer class that can used for visualizing flow direction and magnitude information in meteorology and oceanography raster data.
* WCSLayer is also new at this release and presents raster data from a OGC Web Coverage Service. 

## Widget updates

### Bookmarks - Advanced editing

The 4.17 release brings advanced editing capabilities to the Bookmarks widget - allowing the user to update the bookmark's extent and add thumbnails to their bookmarks. To enable editing on the Bookmarks widget, set the Bookmarks.editingEnabled property to true.

* __Adding and editing thumbnails__: To add or edit a bookmark thumbnail, use the ... button on the thumbnail to access the thumbnail menu. From here, you will have the option to refresh the thumbnail (take a screenshot of the current extent), add a thumbnail from a URL (HTTPS protocol required), or to delete the thumbnail.
* __Editing a bookmark's viewpoint__: When editing a bookmark, the view will update to show the viewpoint of the bookmark. The user can then pan, zoom, or rotate the view, and when they hit Save, the viewpoint of the bookmark will be updated to match the current scale, rotation, and extent of the view.

### Print

The Print widget was enhanced to make working with it easier. First, we added a new exportedLinks property that allows you to access the collection of links exported from a print request. The specifications of the collection is described in the FileLink type definition. We also added two events, complete and submit, to give developers an easier way to manage printing results and behavior.

## WebTileLayer updates

At this release, we updated the urlTemplate property of WebTileLayer to accept {z}/{x}/{y} patterns where z corresponds to a zoom level, and x and y represent a tile location along x and y axis.

## Time support for WMSLayers

You can use the TimeSlider widget to animate your layers over a period of time, or you can set a timeExtent on the layer to only request data that falls within the given time extent. The timeInfo property is set at WMSLayer initialization if the layer has one or more time dimensions.

## LayerView transitions

We enhanced the LayerView in 2D MapViews by adding a fade-in and out transition to the LayerView when we turn the layer's visibility on and off.

## Breaking changes

* For better memory management, view.destroy() now destroys all attached resources, including the map. To prevent the map from being destroyed, you can unset the map before calling destroy().
* StreamLayer.maximumTrackPoints was deprecated at version 4.15 and is now removed. Use StreamLayer.purgeOptions.maxObservations instead.
* The ActionButton and ActionToggle classes now correctly implement their types as either button or toggle. Prior to this, it was set as string.
* The TimeSlider.loop property now has a default value of false instead of true.

## Bug fixes and enhancements

* BUG-000124122: The labels for a VectorTileLayer now correctly overlap when needed.
* BUG-000126549: Fixed an issue where setting NULL field values in the Editor widget would not work as expected.
* BUG-000127825: Fixed an issue where Query.parameterValues were getting dropped from query requests.
* BUG-000128004: Fixed an issue where labels did not print for client-side feature layers.
* BUG-000128560: Fixed an issue where a 3D GLTF model loaded as ObjectSymbol3DLayer would disappear at certain camera angles.
* BUG-000129741: Popup and ListItem actions no longer fire thetrigger-action event when disabled.
* BUG-000129959: Fixed an issue with WMSLayer where some sublayers were not able to be turned off from the LayerList widget.
* BUG-000131005: Fixed an issue where the symbology of the features changes in the Legend when clicking on the map.
* BUG-000131053: Fixed an issue where individual point features did not highlight on selection when clustering is enabled.
* BUG-000131400: It is not possible to create client-side feature layers with a period in the field name.
* BUG-000132100: Fixed a printing isue where Sublayer.title was not honored in the legend.
* BUG-000132263: Fixed the behavior of the CoordinateConversion widget by always displaying a png for the graphic used in capture mode (previously was png or svg depending on the browser).
* BUG-000132335: Clarified documentation for Slider, HistogramRangeSlider, and TimeSlider to indicate the containers for these widgets need a width specified in CSS to render properly in the Expand widget.
* BUG-000133131: Fixed an issue where the TimeSlider would not handle features with infinite start of end times (e.g. startDate: 1/1/2020, endDate: null).
* BUG-000133234: Fixed an issue where the Legend symbols and widget panel size change after clicking on the map when MapImageLayer has sublayers configured with pop-ups.
* BUG-000133521: Fixed an issue where the LayerList widget would not remove layers from a collection.
* GEONET-257043: Fixed an issue where the Bookmarks widget would not allow new bookmarks to be added if view.map was not a WebMap.
* GEONET-257326: Fixed an issue where the ScaleBar widget labels would display on top of other widgets.
* GEONET-947024: Fixed an issue where some widget buttons were missing a type=button property. This only affects button elements when wrapped in a form.
* Fixed an issue where maximumNumberOfFeatures wouldn't be correctly applied in a SceneView.
* Fixed an issue where the ClosestFacilitySolveResult.directions had the incorrect property type. The property is now an array of DirectionsFeatureSet instances.
* Fixed an issue where the WMSLayer was not parsed correctly when the service contained only a root layer, but no nested layers.
* Fixed an issue with popup alignment and docking positions.
* Fixed an issue with the Search widget where the focus remained on the search field instead of the resulting popup after selecting a location.
* ENH-000117307: The ability to load all of the sublayers to a feature service can now be achieved by using the Layer.fromArcGISServerUrl function and pass in the URL to the feature service.
* ENH-000123446: Much work has been done to help speed up general processing and improving symbology update speeds.
* ENH-000123854: The TimeSlider now works with WMSLayers that have time dimensions.
* ENH-000125859: Implemented spatial client-side queries for BuildingSceneLayer.
* Enhanced the printing logic to send GeoJSONLayer as a feature collections instead of a screenshot to the print service.
* Enhanced the CIMSymbolReference typings by adding CIMSymbol.data to help users write correct CIM.
* Enhanced the IntegratedMeshLayer so that modifications are applied without reloading the layer; allowing for smoother transitions between different versions and scenarios.
* Enhanced the ObjectSymbol3DLayer so that the evaluation of visibility is based on the entire shape of the symbol and not just the point (Frustum Culling).
* Enhanced the behavior of the Print widget so that if the end-user modifies the dpi value in the MAP_ONLY layout, then the height and width are automaticlly adjusted. See exportOptions to learn more.
* Enhanced the behavior of the SliceViewModel.tiltEnabled, so that it doesn't automatically start the widget when set.
* Enhanced the behavior of how StreamLayers work when resumed from a suspended browser tab. When a tab is refocused, the StreamLayer resumes and is updated.

## Deprecation

The following are deprecated and will be removed in a future release:

* Use of Internet Explorer 11 is deprecated as of version 4.16. Users are highly encouraged to move to a modern browser such as Mozilla Firefox, Google Chrome, Apple Safari, or Microsoft Edge. While Internet Explorer 11 continues to be supported, use of Internet Explorer 11 is  discouraged, and support will be discontinued in version 4.18.
* decorators.declared deprecated since version 4.16. declared() is not needed to extend Accessor anymore. See Implementing Accessor for updated information.
* ChartMediaInfoValueSeries.x deprecated since version 4.17. Use 'value' instead.
* ChartMediaInfoValueSeries.y deprecated since version 4.17. Use 'tooltip' instead.
* SizeVariable.expression deprecated since version 4.2. Use SizeVariable.valueExpression instead.
* PathSymbol3DLayer.size deprecated since version 4.12. Use PathSymbol3DLayer.width or PathSymbol3DLayer.height instead.
* symbolPreview deprecated since version 4.11. Use symbolUtils instead.
* symbolPreview.renderPreviewHTML deprecated since version 4.11. Use symbolUtils.renderPreviewHTML instead.
* PrintTemplate.preserveScale deprecated since version 4.16. Use PrintTemplate.scalePreserved instead.
* ProjectParameters.outSR deprecated since version 4.4. Use ProjectParameters.outSpatialReference instead.
* RouteParameters.barriers deprecated since version 4.11. Use pointBarriers, polygonBarriers, and/or polylineBarriers instead.
* Bookmark.extent deprecated since 4.17. Use viewpoint instead.
* AreaMeasurement2DViewModel.clearMeasurement deprecated since version 4.16. Use clear instead.
* AreaMeasurement2DViewModel.newMeasurement deprecated since version 4.16. Use start instead.
* AreaMeasurement3DViewModel.clearMeasurement deprecated since version 4.16. Use clear instead.
* AreaMeasurement3DViewModel.newMeasurement deprecated since version 4.16. Use start instead.
* BasemapLayerList.statusIndicatorsVisible deprecated since version 4.15. Use BasemapLayerList.visibleElements.statusIndicators instead.
* BasemapToggle.titleVisible deprecated since version 4.15. Use BasemapToggle.visibleElements.title instead.
* Bookmarks.select-bookmark deprecated since version 4.17. Use bookmark-select instead.
* DirectLineMeasurement3DViewModel.clearMeasurement deprecated since version 4.16. Use clear instead.
* DirectLineMeasurement3DViewModel.newMeasurement deprecated since version 4.16. Use start instead.
* DistanceMeasurement2DViewModel.clearMeasurement deprecated since version 4.16. Use clear instead.
* DistanceMeasurement2DViewModel.newMeasurement deprecated since version 4.16. Use start instead.
* FeatureForm.fieldConfig deprecated since version 4.16. Use FieldElement and/or GroupElement instead.
* FeatureFormViewModel.fieldConfig deprecated since version 4.16. Use FieldElement and/or GroupElement instead.
* FeatureTemplates.filterEnabled deprecated since version 4.15. Use FeatureTemplates.visibleElements.filter instead.
* LayerList.statusIndicatorsVisible deprecated since version 4.15. Use LayerList.visibleElements.statusIndicators instead.
* Popup.featureNavigationEnabled deprecated since version 4.15. Use Popup.visibleElements.featureNavigation instead.
* SliceViewModel.clearSlice deprecated since version 4.16. Use clear instead.
* SliceViewModel.newSlice deprecated since version 4.16. Use start instead.
* Slider.labelsVisible deprecated since version 4.15. Use Slider.visibleElements.labels instead.
* Slider.rangeLabelsVisible deprecated since version 4.15. Use Slider.visibleElements.rangeLabels instead.
* decorators.cast(classFunction) deprecated since version 4.14. Parameter decorators won't be supported by JavaScript decorators.
* Extending multiple classes deprecated since version 4.13. Create mixins with TypeScript and JavaScript instead.
* LabelClass.labelExpressionInfo.value deprecated since version 4.5. Use expression instead.
* SceneView.constraints.collision deprecated since version 4.8. Use Ground.navigationConstraint instead.
* SmartMapping.params.basemap deprecated since version 4.13. Use view instead.
