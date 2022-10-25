# NAME

DataDog::DogStatsd - A Perl client for DogStatsd

# SYNOPSIS

        use DataDog::DogStatsd;

        my $statsd = DataDog::DogStatsd->new;
        $statsd->increment( 'user.login', { tags => [ $s->cs->param( 'loginName' ) ] } );
        $statsd->timing('page.load', 320);
        $statsd->gauge('users.online', 100);

        # namespace version
        my $statsd = DataDog::DogStatsd->new(namespace => 'account.'); # note the '.' at the ending
        $statsd->increment('activate'); # actually as account.activate

# DESCRIPTION

Statsd: A DogStatsd client (https://www.datadoghq.com)

The difference between [Net::Statsd](https://metacpan.org/pod/Net%3A%3AStatsd) and this is that it supports tags and namespace.

# METHODS

## new

- host

    default to 127.0.0.1

- port

    default to 8125

- namespace

    default to ''

## increment

        $statsd->increment( 'page.views' );
        $statsd->increment( 'test.stats', { sample_rate => 0.5 } );
        $statsd->increment( 'user.login', { tags => [ $s->cs->param( 'loginName' ) ] } );

Sends an increment (count = 1) for the given stat to the statsd server.

## decrement

        $statsd->decrement( 'products.available' );

Sends a decrement (count = -1) for the given stat to the statsd server.

## count

        $statsd->count( 'test.stats', 2 );

Sends an arbitrary count for the given stat to the statsd server.

## gauge

        $statsd->gauge('users.online', 100);
        $statsd->gauge('users.online', 100, { tags => ['tag1', 'tag2'] });

Sends an arbitrary gauge value for the given stat to the statsd server.

This is useful for recording things like available disk space, memory usage, and the like, which have different semantics than counters.

## histogram

        $statsd->histogram($stats, $value);

Sends a value to be tracked as a histogram to the statsd server.

## timing

        $statsd->timing('page.load', 320);
        $statsd->timing('page.load', 320, { sample_rate => 0.5, tags => ['tag1', 'tag2'] });

Sends a timing (in ms) for the given stat to the statsd server. The sample\_rate determines what percentage of the time this report is sent. The statsd server then uses the sample\_rate to correctly track the average timing for the stat.

## set

        $statsd->set('visitors.uniques', $user_id);

Sends a value to be tracked as a set to the statsd server.

## event

        $statsd->event('event title', 'event text', { tags => ['tag1, 'tag2'] });
        $statsd->event('event title', 'event text', { tags => ['tag1', 'tag2'], alert_type => 'error'});

Sends an event indicating a specific incident occurring.

Use alert\_type option to specify the kind of event, available options are error, warning, info, and success.

# AUTHORS

Stefan Goethals <stefan@zipkid.eu>
