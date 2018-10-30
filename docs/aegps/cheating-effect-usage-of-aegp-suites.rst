.. _aegps/Cheating Effect Usage of AEGP Suites:

Cheating Effect Usage of AEGP Suites
################################################################################

As soon as we showed developers the initial implementation of AEGP suites, they wanted to "cheat" and use them from within effects. This is certainly possible, but please keep in mind that depending on factors outside the effect API (i.e., any information you get from the AEGP APIs) can lead to trouble. If After Effects thinks an effect has all the information it needs to render, it won’t (for example) update its parameters based on changes made through an AEGP function. We’re actively working on this dependency issue for future versions, but bear it in mind as you write effects which "masquerade" as AEGPs.

Effects can use some AEGP suites to take advantage of camera and lighting information, as well as the `AEGP_GetLayerParentComp <#_bookmark596>`__ and `AEGP_GetCompBGColor <#_bookmark579>`__ functions. This should not be interpreted to mean that effects can use *any* AEGP suite calls. Also, see the `Events chapter <#_bookmark511>`__ for more information on effects adding keyframes.

`AEGP_PFInterfaceSuite <#_bookmark716>`__ is the starting point. The functions in this suite allow you to retrieve the AEGP_LayerH for the layer to which the effect is applied, and the AEGP_EffectRefH for the instance of your effect. `AEGP_RegisterWithAEGP <#_bookmark673>`__ allows you to get an AEGP_PluginID, which is needed for many AEGP calls.

----

Depending on AEGP Queries
================================================================================

One word: Don’t. Effects cannot allow the results of AEGP queries to control what is rendered, without appropriately storing those query results (usually in sequence data), cancelling their own render, and forcing a re-render using the queried information.

This is tricky.

Failure to do so will result in nasty, subtle caching bugs guaranteed to cause hair loss and weight gain.