<script>
    import Course from './Course.svelte';
    import Home from './Home.svelte';
    import Error from '../../components/ui/Error.svelte';
    import Loading from '../Loading.svelte';
    import { Router, Route, navigate } from 'svelte-routing';
    import { createEventDispatcher, getContext, onMount } from 'svelte';
    import { userContext } from '../../contexts/contexts';
    import { getRegisterSteps } from '../../chunks';

    const REGISTER_STEPS = 4;

    let user = getContext(userContext);

    const dispatch = createEventDispatcher();

    let url = '';

    onMount(() => {
        if (!$user.prefs || $user.prefs.registerStep < REGISTER_STEPS)
            return navigate(`/register-steps`);

        if (window.location.pathname.match(/^\/course\/\w{10,30}$/))
            return;

        if (window.location.pathname.match(/^\/home\/[a-z]{1,20}$/)) {
            const path = window.location.pathname.split(/\//g)[2];
            if ([...$user.prefs.classes, ...$user.prefs.specialities].includes(path))
                return navigate(`/home/${path}`);
        }

        navigate(`/home/${$user.prefs.classes[0]}`);
    });
</script>


<Router {url}>

    <Route path="/home/:subject" component={Home}/>

    <Route path="/course/:courseId" component={Course}/>

    <Route path="register-steps">
        {#await getRegisterSteps()}
            <Loading text="Chargement du code"/>
        {:then RegisterSteps}
            <svelte:component this={RegisterSteps}/>
        {:catch error}
            <Error {error}/>
        {/await}
    </Route>

</Router>
